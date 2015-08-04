---
layout: post
title: "Unix as an IDE part 1: Enter Homebrew"
date: 2011-10-05 15:12
comments: true
categories:
- Homebrew
- IDE
---

Newbies to Ruby frequently ask me what IDE I use and they are, for some reason, take aback when I say 'Unix' (or OSX or Linux depending on the crowd).
The response surprises some people a bit which, in turn, surprises me. So, I decided to write this series of posts that describes the basic tools I use
in day-to-day development.  

<!--more-->
###Background
I'm a young guy (1987 or 24 upon writing this post) and I was born into a Java world. For years I plugged away in Netbeans or Eclipse as my IDE
and thought there was nothing else to software development. These integrated IDE's were my life and contained everything I would ever need to 
cut my code. Then again, all I ever did do was write clever game playing programs and single-use swing utilities. Rarely did I venture out into the scary
bash terminal to git, grep, and ack my way through source code. Those were wilder times full of bliss.  

Then I was introduces to Ruby by some guys at [Gaslight Software](http://http://gaslightsoftware.com/) in Cincinnati Ohio and I loved it. I could write
software twice and fast and ten times as sturdy with BDD testing frameworks like [Rspec](http://rspec.info/). So, I downloaded a Text Mate (my first
text editor) and got to work and thus was I introduced to the wonders and bash, Unix, and all it has to offer to coders like myself.

###Homebrew
[Homebrew](http://mxcl.github.com/homebrew/) is the self proclaimed "missing package manager for OS X." I spend most of my time on Apple machines and when
I get a fresh OSX install, the first thing I do is [install Homebrew](https://github.com/mxcl/homebrew/wiki/installation).  

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.github.com/gist/323731)"
```

Note: You will need  

* [Xcode](http://itunes.apple.com/us/app/xcode/id448457090) or the [OSX GCC Installer](https://github.com/kennethreitz/osx-gcc-installer) if you are an advanced user and are looking to save space
* OSX 10.5 or higher  
* Intel Mac  
* [Java Development Update](https://daw.apple.com/cgi-bin/WebObjects/DSAuthWeb.woa/wa/login?appIdKey=d4f7d769c2abecc664d0dadfed6a67f943442b5e9c87524d4587a95773750cea&path=%2F%2Fdownloads%2Findex.action)  

This will install Homebrew to `/usr/local` so you wont need sudo **(never use sudo with homebrew)**.

Now you are ready to rock! Homebrew carries with it list of recipes that can be queried using `brew search`.

###Useful casks
I call installed Hombrew recipes 'casks.' These are applications that are managed by homebrew as if you were using any other package management system like apt-get, macports, or fink.
Here are a few casks that I think are particularly useful.

`brew install git` Git is an excellent source control utility for your code.  
`brew install macvim` A mac GUI for vim.  
`brew install ack` Multi file keyword search for developers.  
`brew install wget` Feature rich terminal utility for retrieving files via HTTP, HTTPS, FTP  
`brew install flip` Script for converting newlines to Windows, Mac, or Unix format  
`brew install python`  
`brew install bash-completion`  
`brew install mongodb`  
`brew install mysql`  

I include mysql and mongodb because, hey, who doesn't need databases amiright?

###Updating casks
So, lets say that you have been using homebrew for a month now and you want to see if you have any out of date casks. First you need to update your
homebrew recipes and check for any outdated casks.

```bash
brew update
brew outdated
```

So, lets say you have several casks out of date. You could run `brew upgrade` on each cask but who wants to do that?! We're coders and the less leg-work we have
to do the better. A quick bash one-liner can get those casks up to date.

```bash
brew outdated | while read cask; do brew upgrade $cask; done
```

###Useful commands

```bash
brew install FORMULA   # Install from a recepie
brew uninstall FORMULA # Uninstall a cask
brew list              # List all available recipes
```
That's all. Homebrew is an excellent package management utility that can act as a foundation for any developer's environment running on OSX.
