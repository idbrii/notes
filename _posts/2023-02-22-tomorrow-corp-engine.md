---
layout: post
title: Time travel, reverse stepping with data breakpoints, scm integration in Tomorrow Corporation's engine
categories: [code, gamedev]

---

[Tomorrow Corporation demonstrated their engine features](https://www.youtube.com/watch?v=72y2EC5fkcE) in a video that I found remarkable.

I've never seen a custom engine that impressed me so much. They've built a whole tech stack so all parts are tightly integrated:

* Custom language, compiler, runtime.
* Custom code editor and debugger.
* Custom game engine (obviously).

Because those parts are custom they can:

* Recompile on every keystroke without visible hiccups.
* Step through rendering code and see individual parts draw to the screen like RenderDoc or Unity's Frame Debugger.
* Reverse step through code
* Run to breakpoint in reverse (or data breakpoint!!!)
* Stores a session file that tracks all of your input and code changes that's portable so you can send to coworkers.
* Retrieve correct source version from version control so when you receive a session, it runs it with the correct code.
* Tag objects with a timestamp so you can jump entire program state to that time for stepping or inspecting.
* Profile, click on spike, click on timestamp on spiking function and it rewinds to that point with the debugger stopped at that function.

I wonder how complex their game can be and maintain good performance with all of this debug enabled. Could they make something with as many moving pieces as Factorio?
