---
layout: post
title: Four Pillars of Vim
categories: [vim]

---


1. Instant access to file- and project-level regular expression manipulation.

1. Easy Programmability.

1. Editing as a language.


# File- and project-level regular expression manipulation.


```vim
nnoremap gs :%s/
xnoremap gs :s/
```

Super charged by these plugins:

* [quickfix-reflector.vim](https://github.com/stefandtw/quickfix-reflector.vim) - Make changes to the quickfix and automatically apply those changes to the original files.
* [vim-notgrep](https://github.com/idbrii/vim-notgrep) - convert vim regex to PCRE (grep-compatible), work with ag/ripgrep/csearch/etc, grep recursively from project root.
* [vim-searchsavvy](https://github.com/idbrii/vim-searchsavvy) - easily start a grep from a vim search, grep your current buffers or args, toggle whole word search on/off.

# Programmability

Vim is remarkably programmable. The threshold between configuration and programming/automation is barely there.

# Editing as a language
