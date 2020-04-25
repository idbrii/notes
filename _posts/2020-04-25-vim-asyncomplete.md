---
layout: post
title: Async completion in vim
categories: [vim]

---

I recently starting using
[asyncomplete.vim](https://github.com/prabirshrestha/asyncomplete.vim) for vim
completion with [LSP
servers](https://microsoft.github.io/language-server-protocol/). I initially
tested it out just to see if it would be more responsive than manually invoking
vim-lsp's completion (which sometimes takes a moment regardless of async
because I don't proceed until it's done) and found that it seemed improve
responsiveness -- presumably because the completions appear once their
available instead of when I request them.

I get a lot more completions than I used to (it completes keywords and short
things I wouldn't bother) and I've had to reconfigure some mappings to handle
frequently exiting from completion. It also [doesn't do fuzzy
completion](https://github.com/prabirshrestha/asyncomplete.vim/issues/137)
(it's prefix only), but invoking vim-lsp's omnicompletion performs fuzzy
completion so that option is always available.

Asyncomplete uses vim jobs to do completion without blocking your input and
it's vimscript, so no dependencies and no compiling. It's by the same author as
vim-lsp, so you can expect pretty solid integration between the two, but
they're also logically separate with the asyncomplete-lsp plugin implementing
an asyncomplete provider for lsp's completions so it's shouldn't be too hard to
switch away from vim-lsp.

When completions have multiple parts -- like arguments in a function --
asyncomplete inserts some placeholders. But if you use Ultisnips, you can
install
[vim-lsp-ultisnips.vim](https://github.com/thomasfaingnaert/vim-lsp-ultisnips)
to make jumping between arguments smoother. I think there are alternatives for
other snippet engines too.

* [async.vim](https://github.com/prabirshrestha/async.vim) - abstract vim's async support
* [vim-lsp](https://github.com/prabirshrestha/vim-lsp) - lsp support
* [vim-lsp-settings](https://github.com/mattn/vim-lsp-settings) - auto configure lsp including server installer
* [vim-lsp-ultisnips.vim](https://github.com/thomasfaingnaert/vim-lsp-ultisnips) - jump between sections of a completion using ultisnips
* [asyncomplete.vim](https://github.com/prabirshrestha/asyncomplete.vim) - autocompletion as you type
* [asyncomplete-lsp.vim](https://github.com/prabirshrestha/asyncomplete-lsp.vim) - integrate with vim-lsp
* [asyncomplete-tags.vim](https://github.com/prabirshrestha/asyncomplete-tags.vim) - integrate with tags files



Then installing the server is usually easy:

    :edit main.py
    :LspInstallServer

You can check if it's working:

    :LspStatus
    :LspDefinition

In short, for vim-plug:

    Plug 'mattn/vim-lsp-settings'
    Plug 'prabirshrestha/async.vim'
    Plug 'prabirshrestha/asyncomplete-lsp.vim'
    Plug 'prabirshrestha/asyncomplete-tags.vim'
    Plug 'prabirshrestha/asyncomplete.vim'
    Plug 'prabirshrestha/vim-lsp'
    Plug 'thomasfaingnaert/vim-lsp-ultisnips'

