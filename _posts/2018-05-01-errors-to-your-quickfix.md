---
layout: post
title: Send errors to your quickfix
categories: [vim]

---

If you want to compile the code you're working with right now, chances are you
can do:

    :exe 'compiler '. &filetype
    :make

and it Just Works. Your code will build/lint/run and you'll get your errors and
output in the quickfix window (`:copen` to see it). Your errors will be
jumpable instead of having to manually navigate to line numbers. This setup
should also work with async make plugins like vim-dispatch or AsyncCommand.

Many filetypes won't work because there's support for multiple different
compilers: C++ could be gcc, msvc, msbuild, etc. However, the `:compiler`
command tab-completes (with 'wildmenu'), so give it a try!

The command is called `:make`, but it works build systems aside from make
(scons, msbuild, etc) and for directly calling compilers or running any
program.

You might not like the 'makeprg' it sets, but you'll probably like the
'errorformat' (to parse the output into the quickfix as jumpable errors).

If you're still not satisfied, try searching github for compiler plugins.
Search for [`compiler
filetypename`](https://github.com/search?l=Vim+script&type=Repositories&q=compiler+java)
and filter by Vimscript language. Or [Click here to open a prefilled advanced
search](https://github.com/search/advanced?l=Vim+script&type=Repositories&q=compiler+FILETYPENAME_GOES_HERE).

I occasionally change the makeprg while keeping the efm:

    compiler python
    setlocal makeprg=python\ -t\ -3\ %

And I use my
[autocompiler.vim](https://github.com/idbrii/daveconfig/blob/master/multi/vim/plugin/autocompiler.vim)
plugin to make it more likely that 'makeprg' and 'errorformat' are already set.


# Stack Traces

You can even setup an 'errorformat' to understand stack traces. I [do that for
python](https://github.com/idbrii/daveconfig/blob/master/multi/vim/compiler/python.vim#L25)
so runtime errors are put into my quickfix. However, 'errorformat' is esoteric
so if you can find someone who's already done it, all the better.

With a proper 'errorformat', you can load your log file with
`cgetfile /path/to/file.log` and then `:cnext` to up the stack trace.


# Fallback Plans

If you can't figure out an errorformat, you can just open your logfile in a
buffer and use `gF` to jump to the file and line under the cursor.

If your stack traces always look like this with no path:

    Exception in thread "main" java.lang.RuntimeException: A test exception
      at com.stackify.stacktrace.StackTraceExample.methodB(StackTraceExample.java:13)
      at com.stackify.stacktrace.StackTraceExample.methodA(StackTraceExample.java:9)
      at com.stackify.stacktrace.StackTraceExample.main(StackTraceExample.java:5)

Then you might have to set 'path' so vim can figure out where to find StackTraceExample.java.
