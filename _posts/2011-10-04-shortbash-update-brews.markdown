---
layout: post
title: "Shortbash: Updating your brews"
date: 2011-10-04 10:12
comments: true
categories: 
- shortbash
- homebrew
- bash
---

I really love homebrew and I especially love updating all my 'casks' recipes using a single command, here's how I do it.

```bash
brew outdated | while read cask; do brew upgrade $cask; done;
```

