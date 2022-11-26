---
layout: post
title: The state of gamedev tools
categories: [gamedev]

---


# Top frequently encountered challenges

## Iteration time

There's a constant battle to reduce iteration time, but each set of tools has
different challenges to keep this time down.

From [Why Focus on Rapid Iteration?](https://www.gamasutra.com/view/feature/132046/building_a_mindset_for_rapid_.php):

> There are usually lots of go-backs and retries as you bring together elements of the game -- the code, content, scripts, etc. -- and see if it's entertaining. In the worst cases, teams don't realize that it's not a fun game until it's so late, they can't fix it.

For Unity, you need to ensure your code (and stuff from the assets store) doesn't break Unity's hot code reloading. [Here's a writeup someone did](https://gist.github.com/cobbpg/a74c8a5359554eb3daa5) on the work required.

For Lua, you need to ensure your class system will pick up changes in live entities (the characters running around in the game should get the new logic). And make sure no new code breaks it.

For C++, you need to figure out a dll unload/load system and ensure it doesn't break your live entities. And make sure no new code breaks it.


## Design Churn

Games tend to be living products during development and lots of things change.
This fluidity is good because we evolve the product towards a better game, but
it's bad because lots of effort gets thrown away. It's also difficult to do
automated testing in this environment because tests become invalid and the
effort setting them up adds to the waste.

These changes in direction often leaves lots of dead code that adds to project
build times, confuses new hires, and slows down future iteration.

Getting through lots of design iteration early to work out these issues before
the feature ramps up to full production helps to mitigate this concern.

## Machine Performance

Games can always run faster. Reduce battery usage for mobile, more common min
spec for desktop, better usage of high spec desktop. We're constantly trying to
cut out milliseconds and parallelize. We build tools to help from analysis
(easy ways to profile before and after) to historical tracking (see trends in
our performance of builds over time). We can use these to improve performance
and catch when we make catastrophic drops.

Some teams are large and have a small group of programmers who understands
performance implications and are constantly helping the rest of the team keep
things under control. Some teams just throw everyone to optimization towards
the end of the project.

Sometimes deadline pressure promotes a prototype to production and its
performance suddenly becomes a problem. Sometimes a new system comes on line
and is over CPU budget, but has so many team dependencies that it has to stay
in the build and there's a scramble to optimize.

The systems/frameworks/engines you work in contribute a lot to this -- from
C#'s garbage collection meaning you're constantly trying to avoid generating
garbage to Unreal's Actor single-threaded system meaning you're finding ways to
disassociate Actors from data so you can thread your processing.

In general, a lot of code out there isn't designed with gamedev in mind -- it
expects a half second delay is fine but gamedev is closer to a realtime system
where you can't have a half second pause between frame updates because each
frame often has a large visual delta to the player and they'll notice small
hiccups.


# SDKs I wish existed

Most desirable elements first (I've actually already built some of these, but would have used existing code if it existed):

* Better linting for Unity (find AWake(), find reasons hot reloading is broken, find slow/unnecessary usages of Resources, find code that will lead to [IL2CPP surprises](https://jacksondunstan.com/articles/4545)).

* Unity asset for localization. Provide a way to define strings, get strings, and load po/csv files for strings. A cool way of doing it is with nested static classes and static strings and use reflection to extract the strings and insert translated ones (but you also need a string key lookup so you can compose keys at runtime).
    * Strong static checking is a plus. (Build-time errors that an incorrect string key was used.)
    * Mostly text-based so I can write my own pipeline logic is a plus.
    * Translators already have poedit and other tools, so it should be trivial for them to continue using those tools.
    * Apparently Unity is [working on their own version this](https://docs.unity3d.com/Packages/com.unity.localization@0.2/manual/index.html).

* System for bug reporting. Capture screenshot, machine info, in-game location, user-provided category, log, user data (arbitrary function returning a string), and upload to a server. Ideally I have the option of running my own instance of that server. Provide aggregation on the server (tag clouds, heatmaps, etc) to help narrow down which problems are most prevalent.
    * Must be GDPR-compliant.
    * Unreal has some parts of the client side built-in with [BugIt](https://docs.unrealengine.com/en-US/API/Runtime/Engine/GameFramework/UCheatManager/BugIt/index.html).

* A python library that provided a single interface for uploading to different storefronts (Steam, Epic, Humble, itch, GOG). I think only some of these storefronts have an API that would make this possible.

* A programmatic UI library for Unity. (Some compromise between their designer-driven approach of building UI prefabs in the inspector vs. the easier to debug and re-use programmer approach of defining functions to make re-usable UI elements.)

* Performance test and analysis. Users instrument code sections and specify CPU millisecond budgets for those sections. It runs tests, captures profile, and attributes budget expenditures to authors by cross-referencing profile with changed files. Must issue warnings for both over budget and massively under budget (often means something accidentally disabled). Should provide a chart to compare average CPU usage across test runs (commit ranges).


# Building/packaging

Steam provides a [command-line tool for uploading builds called
steamcmd](https://partner.steamgames.com/doc/sdk/uploading), but you still need
to do a bunch of scripting to make it work with build systems and it doesn't
have a solution for generating vdf (which are the data definitions that
steampipe consumes).

We had to write a wrapper around steampipe that would copy a completed build
artifact from jenkins, unzip it, figure out the changelist, generate a vdf, and
call steampipe with the appropriate arguments (including the detected
changelist). Parts of this wrapper code are portable to other steam games or
tools, but not to other storefronts.

I don't think I can talk more specifically about problems due to NDA.


# Version control

We use svn. [See my feelings back here again.](https://www.reddit.com/r/gamedev/comments/9l0b59/what_do_you_use_for_version_control_for_art_files/e73q7p0/)

In the past I've used Perforce. It has most of the same problems as svn, but
feels more polished and doesn't have bugs when branching. I've used it on much
larger projects (200 people on perforce vs 100 people on svn) and it seemed
prone to slowdown (possibly because someone else was doing something that was
occupying the server). The centralized model and poor vim integration make
both much less desirable than git. But at least they handles large files well.

Perforce also has some nice tools. The p4merge in p4v works to diff images in
fancy ways (so does the diff in TortoiseSVN). I haven't found a tool as good as
the timelapse view in p4v. Blame in TortoiseSVN isn't nearly as easy to
navigate backwards *and forwards* and git tools I've used aren't so fancily
graphical.

Perforce is used by all large gamedev studios I've worked for.


For diffing, I've used Beyond Compare, Araxis Merge, WinMerge, p4merge, Meld,
and vimdiff. Beyond Compare is really great and has two big benefits: the
ability to script it is fantastic and the ability to set the current line in
both files is super helpful. But recently I just use vimdiff a lot and haven't
looked back (but I do less merging now than I used to).


# QA

I wrote some code for bug reporting:

* Capture screenshot, machine info, in-game location, bug category, log.
* Prompts player for description.
* Sends to a server and generates a dev-only forum post.
* Bug reports have a location field that can be pasted into Unity to jump the camera or player to that location and camera facing direction.

Both players and QA contractors use this system from within the game.
Drastically includes the quality of bugs when they always have a log, location,
and screenshot.

One of our other games has a report bug button that also includes your save
file in the report and posts the bug report in a Slack channel.

