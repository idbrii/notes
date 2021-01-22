---
layout: post
title: Enums in data instead of code
categories: [code, gamedev]

---

# Summary

Use instances of a data object to enumerate different values and use
the implementation of the object to perform comparisons between data instances
or implement behaviour.


# The Backstory

While working on a grid-based movement prototype, I wanted to define a way to
define which units could occupy the same cell. I was reminded of a technique I
used on a previous project and thought I'd write about it. This technique
partially stems from back in the day when updates to console games had
limitations. Patching your game (typically a new .exe) cost you money (paid to
first party -- the console manufacturer), but you could release new data in the
form of DLC at no additional cost. So if you can write your code in a way where
you can add new weapons, powerups, levels, etc entirely by adding new data
objects, then you can save a great deal. Additionaly, since the executable
hasn't changed, there's lower risk of introducing bugs to the parts of your
game that already went through extensive QA.


# The Use Case

In a game, you often have a variety of uses for enums. In this case, let's look at factions. We have many entities and want to assign them to a specific faction and define which faction


See also this great artcile on using [enums, flags and bitwise
operators](https://www.alanzucconi.com/2015/07/26/enum-flags-and-bitwise-operators/)
in C#.
