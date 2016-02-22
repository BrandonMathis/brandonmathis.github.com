---
layout: post
title: "Shorten Capistrano Revisions"
date: 2012-01-04 13:20
comments: true
categories: 
- capistrano
- ruby
- rails
---

So, if you are like me then you like for the code on your production server to take up as little space as nessisary. Also, you like capistrano.
Lets face it, using a simple `cap deploy` to fire off your code to production or staging is awesome but capistrano likes to leave behind old
revisions of your code each time you deploy. So, often I find myself having to run a `cap deploy:cleanup`. This can get annoying so a found a 
clever way to make capistrano clean up after itself after each deploy.

~~~ ruby
after :deploy, 'deploy:cleanup'
~~~
