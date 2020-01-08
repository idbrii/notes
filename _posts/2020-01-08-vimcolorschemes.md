---
layout: post
title: Writing vim colorschemes
categories: [vim]

---

A vim colorscheme determines how all UI elements are coloured. Try `:help
group-name` and you'll see the colours used for each kind of token in your
code. With your current colorscheme, you should recognize the colours you like
displayed with vim's name for those tokens (the left column).


# Merging colorschemes

If you like different parts of a few colorschemes, you can copypaste bits of
them together. First, you'll need to understand the syntax items you're looking
at. Use this (if you have a fancy statusline, turn it off with `:AirlineToggle`
or equivalent):

    :let &statusline = 'ascii:%-3b hex:%2B %{PrintNumSelected()} %= syntax<%{synIDattr(synID(line("."),col("."),1),"name")}> color<%{synIDattr(synIDtrans(synID(line("."),col("."),1)),"name")}> %l,%c%V %P '

Now you'll see 'syntax' and 'color' in the bottom right of your statusline.
'syntax' is the name your syntax uses for the token under the cursor (defined
in `syntax/c.vim`) and 'color' is what colour to select from the colorscheme
(group-name).

Put your cursor on the word `int` in a C++ file and you'll see something like:

    syntax<cType> color<Type>

In your colorscheme you'll see code like this:

    hi Type	guifg=darkkhaki

That tells vim to use 'darkkhaki' to colour that `int` keyword. 'darkkhaki' is
an X11 colour name, but you could use the hex code instead: `guifg=#bdb76b`.


# Writing your own colorscheme

Colorschemes are mostly comprised of `:highlight` commands like these:

    highlight Search guifg=SkyBlue guibg=grey10 gui=none
    highlight Search ctermfg=grey ctermbg=blue cterm=bold,underline

Arguments usually prefixed with 'gui' are for gvim and with 'cterm' for vim in
colour terminals (for more, see `:help highlight-args`).

Keep in mind that `:highlight` *adds* to the highlight definition. So if you do:

    hi IncSearch	guifg=grey10 guibg=SkyBlue

you'll have a blue foreground and grey background because IncSearch has
`gui=reverse` by default. Instead, you'll need to explicitly clear the
highlight-args:

    hi IncSearch	guifg=grey10 guibg=SkyBlue gui=none


# Writing a colorscheme with broad support

There are lots of details to think about when making a colorscheme for others to use:

* will you support nonstandard group-names?
* make it work in term, cterm, gui?
* required terminal palette settings to look correct?
* true color, 256 color, 16 color support?
* fancy statusline theme support ([airline](https://github.com/vim-airline/vim-airline-themes), [lightline](https://github.com/itchyny/lightline.vim/tree/master/autoload/lightline/colorscheme))?

[vim-colortemplate](https://github.com/lifepillar/vim-colortemplate) is a
plugin that resolves *some* of these issues. It generates colorschemes that
look like
[wwdc16.vim](https://github.com/lifepillar/vim-wwdc16-theme/blob/master/colors/wwdc16.vim).
