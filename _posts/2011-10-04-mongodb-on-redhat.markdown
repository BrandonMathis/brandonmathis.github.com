---
layout: post
title: "Mongodb-on-redhat"
date: 2011-10-04 10:20
comments: true
categories:
- redhat
- mongodb
- bash
---

So, I had a recent project that required me to setup two [MongoDB](http://www.mongodb.org/) servers on RedHat with one acting as primary and the second acting as a replica set. Setup and installation of MongoDB is a easy as cake, here's how I did it.

<!--more-->

Start with a basic RedHat server with yum installed. I went with Red Hat Enterprise Linux Server release 5.4 (Tikanga)

###Step 1
[EPEL]( http://fedoraproject.org/wiki/EPEL ) (Extra Packages for Enterprise Linux) is a cool Fedora special interest group that keeps a collection of high quality packages for enterprise linux. [Download](http://fedoraproject.org/wiki/EPEL#What_packages_and_versions_are_available_in_EPEL.3F) an RPM of EPEL for the version of whatever system you are running on and unpack it (if you have not done so on your system already).

```sh
wget http://download.fedoraproject.org/pub/epel/5/i386/epel-release-5-4.noarch.rpm
rpm -Uvh epel-release-5-4.noarch.rpm
```

###Step 2
Once you have finished that portion of the setup, you are ready to install MongoDB. The gist bellow shows a few easy steps that worked for me to get mongodb up and running with minimal config work.

```sh
yum install mongodb-server
mkdir /var/lib/mongodb
chown mongodb:mongodb /var/lib/mongodb
/etc/init.d/mongod start
```

This gist will
<ul>
	<li>Get the server version of the MongoDB package with yum</li>
	<li>Create the mongodb file</li>
	<li>Give ownership of that directory to the newly created mongodb user (created when installing the mongodb-server package via yum)</li>
	<li>Start the daemon</li>
</ul>

###Step 3
In order to access the db from an external resource, you must allow access via iptables.
Open /etc/sysconfig/iptables using your favorite text editor and add the following line before COMMIT:

```
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 27017 -j ACCEPT
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 28017 -j ACCEPT
```

<strong>This file is a table of rules, the first rule has precedence. If the first rule dis-allows everything then nothing else afterwards will matter. It is typical for the last line in a iptabels file to be a rule that disallows all access. Make sure your new rule comes before this rule.</strong>

This will open up access to port 27017 and 28017 on your machine. 27017 is the port mongodb runs by default. 28017 is the mongodb admin console port and is always the port 1000 higher than the port the database is running on. 
If you run mongodb on a custom port then adjust the iptables entry accordingly (ie: replace 27017 with the port you chose and 28017 with the port you chose +1000). 
Also, it is important that you make sure that this rule sits 

###Step 4
Restart iptables

```sh
/etc/init.d/iptables restart
```

Keep an eye out for my post where I detail how I [setup two databases as a replica set](http://www.mongodb.org/display/DOCS/Replica+Set+Tutorial).
