---
layout: post
title: "Hello Meteor with Coffeescript"
date: 2012-04-15 16:04
comments: true
categories:
  - meteor
  - coffeescript
  - javascript
---

[Meteor]( http://meteor.com/ ) is a javascript framework that is worlds apart from
any web framework I have worked with before! If you aren't familiar with Meteor then I
suggest that you take a few minutes to watch the screencast bellow that explains the
framework better than I could in this post!

<iframe src="http://player.vimeo.com/video/40104996?title=0&amp;byline=0&amp;portrait=0" width="570" height="300" frameborder="0" webkitAllowFullScreen mozallowfullscreen allowFullScreen></iframe>

Interested now? If you are then read on to see how I got meteor working with coffeescript!

<!--more-->

## First Meteor App

First things first. We need to get meteor up and running. Now, as of this post there
is no homebrew recipe for meteor but there is a [pull request](https://github.com/mxcl/homebrew/pull/11586).
If you want to see that happen then please contribute! Until them, we can use the 
snippet show in the screencast to install meteor

### Install Meteor

~~~bash
>: curl install.meteor.com | /bin/sh
~~~

Once you have meteor installed you can create a skeleton by running the `meteor create` command

### Create Your App

~~~bash
>: meteor create FirstApp
~~~

Now, you have your app created but I like to use coffeescript. Well, meteor has the ability to
install small packages that can give you customized and extended functionality so we will be
installing the meteor coffeescript package.

### Install Meteor's Coffeescript Package

~~~bash
>: meteor add coffeescript
~~~

This will install the coffeescript package to meteor and add 'coffeescript' to you .meteor/packages
file inside of you app's directory!

### FirstApp.js to FirstApp.coffee

You will need to change the file extension of your FirstApp.js file to .coffee. Altogether,
your app should look like this

~~~coffeescript
root = global ? window
 
if root.Meteor.isClient
  root.Template.hello.greeting = ->
    "Welcome to FirstApp."
 
  root.Template.hello.events = "click input": ->
    alert "You pressed the button"
~~~

That is all the code you need! Meteor automatically serves us the js libraries you need to make the
app run along with libraries like JQuery and Underscore!

### Fire Up the App

~~~bash
>: meteor
~~~

That is all you need! Meteor will now be running on `http://localhost:3000`. Open your browser there
to checkout your app. All changes to your code and automatically hot-deployed to all running instances.
So, I encourage you to try this out and contribute. Who knows where something like this could take us!
