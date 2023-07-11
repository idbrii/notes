---
layout: post
title: Requirements for a bug reporting system
categories: [gamedev, code]

---

Someone asked me about what I'd want in a bug reporting system, so I compiled
those thoughts here. I've written parts of these systems myself and certainly
used them a lot, so hopefully this is a decent spec.

Basic requirements for a bug reporting system:

* Capture screenshot, machine info, in-game location, user-provided category, log, user data (arbitrary function returning a string), and upload to a server. Ideally I have the option of running my own instance of that server.
* Provide aggregation on the server (tag clouds, heatmaps, etc) to help narrow down which problems are most prevalent. **Must be GDPR-compliant.**

----

For a general system, it's important to think about how different games have different needs and figuring out how to provide the most value to your target market:

* lots of worldgen makes some kinds of data (location) useless.
* static world makes other data (location) very useful.
* logs may be enormous (GB) and full of useless garbage. useful info is usually at beginning and end, but sometimes in the middle (followed by spam caused by the error).
* iOS has limited ability to access log files so a log buffer may be useful (was for us). Create a circular buffer and add each log print so they're in memory.
* some platforms already have bug reporter that may access data that you cannot (like crash dumps) -- how to get those into your system too.

# What data should it upload?

* date
* platform/proprietary user id
* game version: build type (editor/debug/release/optimized), steam beta branch, revision or commit hash
* hardware: CPU, GPU, RAM
* OS version
* game world location (name of level, camera/player position, camera direction) -- enough to programmatically jump back to that exact location
* log file
* screenshot
* provided by player: summary/title, description, category (Gameplay, Graphics, etc)
* save file
* world seed (for procgen)
* callstacks for scripting errors

Very specifically, the hardware info we grab [using Unity's SystemInfo](https://docs.unity3d.com/ScriptReference/SystemInfo.html):

* Environment.OSVersion.Version
* SystemInfo.graphicsDeviceName
* SystemInfo.graphicsDeviceType
* SystemInfo.graphicsMemorySize
* SystemInfo.operatingSystem
* SystemInfo.processorType
* SystemInfo.systemMemorySize

# What rate of bug reports do you expect?

We got 20 bugs last week. Looks like lots of different people, but some people send better bugs (more details, repro steps) and some send unhelpful bugs (just a screenshot with a title "where collectible?"). Some of the bugs are in other languages (our game is in 11 languages). We probably get lots of feedback about the same problem, but don't any kind of automatic categorization (no heat maps, word similarity, etc).

I don't have any tracking for who submits reports (like how many were reported by one user), but I have seen some names pop up multiple times.

I wish we had fancier tools to make it easier to recognize issues that are affecting a lot of players to help prioritize.

# Ways to present bug reporter

* Pause menu has a report bug option that opens an in-game screen for player to fill in title, description, and includes a screenshot of whatever's behind the pause screen.
* Press a key from anywhere to open bug report screen with screenshot. Nice because it works for bugs on screens, but users have to remember that hot key.
* Console command that brings up a bug report screen (like BugIt in Unreal).
* External tool. Usually for internal use only.

# Where to store bug reports

* Forum - as posts in a specific sub forum
* Slack - in a dedicate channel
* Bug tracker - like the open source [redmine](https://www.redmine.org/) 

In theory, users can comment on their bugs, but it's much easier to report another bug.

Beware making users feel they can check the status of their bugs if you don't intend to keep the status updated.

