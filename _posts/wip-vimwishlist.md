---
layout: post
title: My Wishlist for Vim
categories: [vim]

---

# 'shellslash' -- separate slash direction from quoting method

When using vim across both Unix and Windows, it's convenient to set
'shellslash' to allow a consistent key for path separators. Doing so also
avoids issues where vim (or plugins) treat backslash as an escape character
(#1274). However, the 'shellslash' option also assumes that shells using
forward slashes use unix-like quoting rules.

cmd.exe handles forward slashes in many cases and works reasonably well as a
shell when combined with mingw or cygwin for unix utilities. It has the added
benefit that it supports native Windows executables that don't have unix ports
(notably Microsoft compilers).

In practice, using 'shellslash' with cmd breaks plugins that use shellescape
(e.g., netrw, vcscommand,
[signify](https://github.com/mhinz/vim-signify/issues/15#issuecomment-15529339)).

Could we separate the quoting method option from the slash direction option? As
far as I can tell, that's not what the shellquote option does?

https://groups.google.com/forum/#!topic/vim_dev/0UIwGZraNfs

This is on :h todo.
