---
layout: post
title: Thoughts on Vim emulation in Chrome
---

I tried out three vi emulator extensions and these are my notes.


# SurfingKeys
Config is javascript, so it's more like real programming. To remap keys, you
`map('x', 'y')` where 'y' is whatever predefined thing you want -- no helpful
labels.

Only displays hints for links that are visible.


# cVim
Seems really complex. There's a ton of options.

Config is a pseudo-vimscript.

It displays EasyMotion hints for links that are not visible.

# Vimium
Vimium seems simpler than cVim.

Config is a series of map statements to bind keys to commands. There's also advanced config.

Can't see how to configure EasyMotion. It displays hints for links that are not visible.
