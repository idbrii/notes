---
layout: post
title: Running Playtests
categories: [gamedev]

---

We're a medium sized studio, so I'm not sure how this translates to other
games, but hopefully it's helpful.



We posted about our playtest on Steam news and our forum a week before the
playtest date. We opened 1k slots at first to see if the game would run well
for everyone. We slowly let more people in over a few days until we opened the
floodgates and allowed everyone who signed up. This roll out seemed to work
really well for us -- we fixed some early issues and had very positive
response.



We tried to take feedback as data and not action items (this is hard!). We have
a hard game and a lot of feedback was about challenge, hitting timing, or sent
immediately after failure. Sometimes "this enemy is cheap" feedback meant
"good, they're hard".

Advice from one of our senior designers: Beware of overpolishing the beginning.
Keep players long enough so they can make an informed decision whether the game
is right for them. If they leave, that's okay. Lots of players see a game and
hope it will be X and when the game is Y, they ask for changes. We had to
exercise restraint. As a gamedev, you often get the most feedback on the
beginning of the game and it's easy to use new player feedback to polish it
into something less interesting ("cheap" enemies in Dark Souls, complexity of
Factorio, ...).

We have forums/discord where players discussed the game, but our in-game
feedback tool doesn't post feedback publicly. This reduces any urge to respond
to feedback and let the dev team discuss freely.

Our playtest build was very limited -- we only included our best content to
focus feedback and put a good foot forward. The build included pretty much zero
meta game and was really just about combat.



Watch out for the team delegating risk evaluations. We made a risky change and
it broke the game and then patched it and broke it differently. Afterwards,
several team members (including myself) said we regret not speaking up about
that change sooner -- we thought someone else had considered the consequences.



People are likely to figure out how to keep playing your game after you take
down the playtest. We still get crash reports.



People might try to mod your playtest! They dug around in our files and changed
lua code to adjust the language, unlock new areas, and change behaviour. We
received many bogus automated crash reports from modders. Probably not a
problem if reports are all manual.

To help with the "feedback on the start" problem, we created a debug cheat to
skip through a specific block of quests to put you after the tutorial and
another to go deeper in to the game. But we did that after the playtest.


