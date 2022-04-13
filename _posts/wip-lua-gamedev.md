---
layout: post
title: Love of Lua
categories: [gamedev]

---

I had a hard time getting into Lua when I first tried it. I was just trying to
write some test code just like when learning other languages. I played [Hack
'n' Slash](https://store.steampowered.com/app/246070/Hack_n_Slash/) but didn't
understand Lua enough to have fun digging through their code to get deep into
hacking the game.

Then I worked on my first Lua game (C++ engine, Lua scripting) and **I was
hooked**. 

## Tables

First, **dictionaries are so useful** and it's awesome how trivial they are to
use compared to verbose syntax and requirements in many other languages --
especially for dictionaries containing different types. Think about how many
problems you solve in other ways because a dictionary isn't as performant or
simple? What if it was both?

## Hot Reload 

Second, **hot reload actually works** and is usually instant.
([lume](https://github.com/rxi/lume) has one you can adapt, I use
[gabe](https://github.com/Alloyed/gabe)'s class system and reload since it's
already integrated). Since an instance of an object is a table, and functions
on the object are elements in a table, you can swap out functions for their new
values and keep your current state. By comparison, Unity's C# hot code
reloading requires you to serialize your state because it needs to unload the
AppDomain. It needs to rebuild the world with the new types. Most serialization
occurs automatically, but often it doesn't and you need to [add special
callbacks to make it
work](https://docs.unity3d.com/Manual/DomainReloading.html). Regardless, for
projects of any real size, it's slow. Not sure how Unreal's Live++ (Live
Coding) works, but seems like [you can't edit .h
files](https://unrealcommunity.wiki/live-compiling-in-unreal-projects-tp14jcgs).

## "standard library"

Lua is a small language and its "standard library" is very minimal. This was
one of my initial roadblocks. Lua's intended for embedding so usually the host
program provides a broader standard library by exposing functions to lua.
However, there are several standard library packages for lua:
[batteries](https://github.com/1bardesign/batteries),
[Penlight](https://github.com/lunarmodules/Penlight), or the aforementioned
[lume](https://github.com/rxi/lume). I'd definitely recommend checking these
out to help get closer to functionality level of most other languages (I use
both lume and batteries, but dropped penlight awhile back).

One of the weirdly nice things about the minimal standard library is that
you're in full control of how things work. Want to change the class system? Go
for it! Prevent writing to the global environment? Sure!

## Automation

I still don't think lua compares well to python for automation tasks like
processing images, scripting build pipelines, etc. But I have decades of python
experience against ~5 years with lua.


## Thanks for coming to my TED talk

Lua's great, but it's not the complete solution. We have a lot of our game
logic in Lua, but all the rendering logic is in C++. In my love2d projects,
that engine is written in C++ and all the code I write is lua.

I also really like python so I my brain is definitely wired to work well with
dynamic languages. Some people find large codebases untenable without static
typing, so YMMV. I left that lua game to work for a few years on a Unity
project and am now happy to be back in the Lua pond.

[ref](https://www.reddit.com/r/gamedev/comments/u2m6m6/thoughts_on_lua/)
