---
layout: post
title: "Automated EC2 Snapshots/Backups"
date: 2013-11-07 11:05
comments: true
categories: 
- ec2
- ruby
- devops
---

I do a lot of work directly with EC2 setting up and tearing down instances and, for some times now, I've been wishing for some kind of automated backup system for all my instances and their volumes. There are a few sites that will provide this service for a fee but I thought it would be pretty trivial to just implement this on my own.

##Basic Requirements
To get this working you will need a few things:
* A server that is always up, I use our Ubuntu CI Jenkins server.
* Amazon's EC2 API Tools
* A set of security credentials for your account
* And, as always, Ruby :)

##EC2 API Tools Setup
Create an X.509 Certificate for this machine at the My Account -> [Security Credentials](https://portal.aws.amazon.com/gp/aws/securityCredentials) page. Once you have the certificate and key, scp those up to your server

~~~bash
scp <path-to>/cert-*.pem ubuntu@your.server.com:
scp <path-to>/pk-*.pem ubuntu@your.server.com:
~~~

Once that is done, ssh onto your server.

First, ensure you enable [Multiverse](https://help.ubuntu.com/community/Repositories/CommandLine#Adding%20the%20Universe%20and%20Multiverse%20Repositories). The simplest way to do this is to just open your `/etc/apt/sources.list`. More than likely, the lines that enable multiverse just need to be commented out.

###Update apt-get.

~~~bash
sudo apt-get update
~~~

###Install amazon ec2-api-tools.

~~~bash
sudo apt-get install ec2-api-tools
~~~

If you're running an old version of Ubuntu then you may want to make use of awstools ppa in order to get the most up to date version of ec2-api-tools

~~~bash
sudo apt-add-repository ppa:awstools-dev/awstools
sudo apt-get update
sudo apt-get install ec2-api-tools
~~~

###Add the following to your .bashrc

~~~bash
export EC2_KEYPAIR=<Your keypair name> # name only, not the file name
export EC2_URL=https://ec2.<your ec2 region>.amazonaws.com
export EC2_PRIVATE_KEY="$(/bin/ls "$HOME"/.ec2/pk-*.pem | /usr/bin/head -1)"
export EC2_CERT="$(/bin/ls "$HOME"/.ec2/cert-*.pem | /usr/bin/head -1)"
export JAVA_HOME=/usr/lib/jvm/java-6-openjdk/
~~~

###Load those changes into your environment

~~~bash
source ~/.bashrc
~~~

###Check to see if everything is working.

~~~bash
ec2-describe-images -o self -o amazon
~~~

##Use Cron to create automated backups

I used the following script, executed by cron, to make backups once a week on sunday

~~~ruby
VOLUME   = /ATTACHMENT\s+(vol-\S+)/
SNAPSHOT = /SNAPSHOT\s+(snap-\S+)/
SNAP_MAX = 10

def get_volumes
  results =[]
  %x[ec2-describe-volumes].each_line{ |s| results << s }
  results.map! { |x| x.match(VOLUME)}
  results.compact!
  results.map!{ |x| x[1] }
end

def get_snapshots
  results =[]
  %x[ec2-describe-snapshots].each_line{ |s| results << s }
  results.map! { |x| x.match(SNAPSHOT)}
  results.compact!
  results.map!{ |x| x[1] }
end

def trim_snapshots(snaps)
  return true if snaps.length < SNAP_MAX
  %x[ec2-delete-snapshot #{snaps[0]}]
  trim_snapshots get_snapshots
end

vols = get_volumes
snaps = get_snapshots

trim_snapshots snaps

vols.each{ |vol| puts %x[ec2-create-snapshot #{vol}] }
~~~

[Here](https://help.ubuntu.com/community/CronHowto) is an excellent resource for how to use cron.
