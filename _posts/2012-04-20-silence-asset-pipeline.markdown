---
layout: post
title: "Silencing the Asset Pipeline"
date: 2012-04-20 14:27
comments: true
categories: 
- ruby
- rails
---

I'll admit, I don't love the asset pipeline. It's difficult to test, debug or understand.
It is also very noisy. I frequently find myself scrolling through log output from the
asset pipeline. Or, I did, until I found out about the following techniques you can use
to silence the asset pipeline forever.

```ruby Gemfile
gem 'quiet_assets', group: :development
```
