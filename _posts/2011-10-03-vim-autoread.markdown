---
layout: post
title: "ShortVim, Autoread"
date: 2011-10-03 15:21
comments: true
categories: vim
---

This should be in everyone's .vimrc  
No exceptions!!  

Automatically updates a file being viewed in vim when it is updated outside of vim.  

```vim
:help autoread

" When a file has been detected to have been changed outside of Vim and
" it has not been changed inside of Vim, automatically read it again.
" When the file has been deleted this is not done.  |timestamp|
" If this option has a local value, use this command to switch back to
" using the global value:
```

Just add to your .vimrc file

```vim
set autoread
```
