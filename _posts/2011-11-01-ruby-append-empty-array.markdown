---
layout: post
title: "Appending Values to Arrays"
date: 2011-11-01 16:52
comments: true
categories: ruby
---

Often, I find myself working with some accessor or variable whose contents I do not know. When I need to append some value to these,
I use a bit of ruby code that I consider to be quite clever.

``` ruby
(a ||= []) << "a"
# => ["a"]
```
