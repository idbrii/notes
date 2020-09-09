---
layout: post
title: Designing for Re-entry: Getting players back into the game
categories: [gamedev, design]

---

I saw this [thread about let's plays and long games](https://www.reddit.com/r/gamedev/comments/8sjaur/developers_say_twitch_and_lets_plays_are_hurting/e102a7d/), and I wanted to write about some specific solutions.


# Problem

People with small amounts of spare time have a hard time getting back to long
narrative games. Games tend to be designed to be a single long experience and
often have few affordances to redoing their onboarding.

Lets explore some ideas for improving the "haven't played in a month" player experience.

# Successful examples

* the [Previously on Alan Wake](https://www.youtube.com/watch?v=dCfUE5wBkE0) is effective: it summarizes what happened before and the character's current motivations and it's good opportunity for character-building. You get one between each chapter which makes them a great stopping point. (This is very expensive and needs to be integral to the game.)

* [Deus Ex: HR's loading screens](https://youtu.be/dwZsXHCCfs4?t=19s) summarize where you are in the quest (with multiple updates for each quest). It's different information each time you load (assuming you made progress), but not information overload. (This is pretty cheap.)

* [Arkham City uses screen shots](https://www.mobygames.com/images/shots/l/573259-batman-arkham-city-windows-screenshot-typical-loading-screen.jpg) on the loading screen to lay out objectives or summarize recent plot points.

* [Dishonored 2's Travel Log](https://miro.medium.com/max/2000/1*qJT35zAc8KSl4z16AKpv7g.png) documents the player's journey with notes and photographs. This journal [update after each mission](https://dishonored.fandom.com/wiki/Travel_Log_(Dishonored_2)). Dishonored 2 also features a pinboard connecting the characters/locations to give the player some spatial overview of what’s progressing in the world.

* Persona 5 has a few methods: a [persistent countdown timer in the top right](https://youtu.be/XZEkg_r5PDw?t=264) to highlight your deadlines, frequently mention objectives in dialogue, show snippets of conversations and tweets (usually related to the objective), dungeon save points trigger companions to talk about the next dungeon objective. 
<!-- [The UI Design of Persona 5](https://jiaxinwen.wordpress.com/2017/04/27/the-ui-design-of-persona-5/) seems to be a good breakdown, but doesn't highlight any of these features. -->

# Potential Solutions

On a recent game, I pitched the loading screen as a mess of tweets about recent in-game accomplishments and plot points. It was age-appropriate to one of the characters, could convey character-building, and I thought a mix of pictures and text would be effective for keeping it fresh. (This is a bit expensive, but might be worthwhile for a long game.)

I'd like to try a tutorial pop up that occurs the second time you do an action unsuccessfully (the initial teach) and every time you fail three times in a row. So if you need to mash X during a grapple (in a scarce-UI scarce-tutorial game) and you keep forgetting, then the game can tell you what's going wrong. If you come back after a long time, the game can remind you how to play. Need a careful balance between the game taunting the player on each failure. This logic can also push tips into the loading screen. (Probably worth the effort if avoiding hand holding tutorialization.)





Thanks to contributions from PiLLe1974 and Swiftster [in the original reddit thread](https://www.reddit.com/r/gamedev/comments/8sm4qi/ideas_for_designing_reentry_to_your_game/).
