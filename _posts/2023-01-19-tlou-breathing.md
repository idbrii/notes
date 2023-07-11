---
layout: post
title: Audio\: The Last Of Us Part II’s Breathing System
categories: [audio, gamedev]

---

I thought [Beau Anthony Jimenez](https://www.beaujimenez.com/)'s thread about this Breathing System really incredible attention to detail.

* twitter thread: [part 1](https://twitter.com/thebeauanthony/status/1284544387616104448) and [part 2](https://twitter.com/thebeauanthony/status/1284544453709934592)
* unrolled version: [part 1](https://threadreaderapp.com/thread/1284544387616104448.html)
and [part 2](https://threadreaderapp.com/thread/1284544453709934592.html) (text is more condensed and includes videos that are omitted here).


I find twitter hard to read (and it's no longer open for all), so here's the text (but it's missing all the important videos with audio).


----

## Breathing System Thread Part 1

One gift of working at Naughty Dog is that the sky is limit. With artists and programmers collaborating closely, magic can happen. The Breathing System is an example of that magic.


There are two fundamental concepts to understand with this system: Murmuration and Heart Rate.

Murmuration is a looping sound on a character. It's the lowest priority sound
possible, so any sound that triggers (from melee, anim, script or code) will
stomp it. After the oneoff plays out, the loop will return where it left off,
creating a seamlessness in the breath.

It's dubbed Murmuration because it's 'the sound under the sounds'. For Ellie &
Abby it's immersive breathing, for Clickers it's clicking, shrieking or
frenzy-ing, and for dogs it's lick-lipping/snorting/etc. It's anything we need
it to be, and that's the power behind it.

Heart Rate is a value expressed as a number: 0.0 to 1.0. It is an encapsulation
of what a character's Heart Rate would be in given context, with 1.0 being the
most exerted and 0.0 being the most relaxed. This 'heart beat' exists for every
Player character, Buddy character, Infected, Dog, and Human NPC in the game.

To measure the Heart Rate, there are states that I assign numerical values to
called Heart Rate States. These states can be based on AI awareness (like
Unaware), enemy attack type (like Bloater Charge), or anything needing a
looping solution.

Programming was able to split out as many states as I needed to get the job
done. For example, Clickers have states that include Frenzy and Unaware, which
have a target Heart Rate of 1.0 and 0.7 respectively. This gives me the ability
to be able to play a different looping sound based on all sorts of game
context.

In this thread, I want to exclusively talk about Ellie, one of the player
characters. Ellie has Murmuration enabled, so she's always playing a sound
loop. She has several Heart Rate States that are split by tension and whether
she's sprinting or not.

As an example, if Ellie is sprinting in ambient tension, her Heart Rate state
is 'ambient-high'. When you let go of sprint, (NOT sprinting) it becomes
'ambient-low'. With this, I'm able to determine how long it takes the Heart
Rate number to go up/down from different states, letting me choose how long I
want to hear Ellie's exhausted breaths peter out or how long it takes Ellie to
sound worn-out after sprinting for a period of time.

The possibilities become endless. I am able to do things like:

1. Play different sounds based on how long you've been sprinting, in any tension.
2. Play various stages of exhausted breathing after you've been sprinting for certain amount of time.

If Ellie starts sprinting when the enemy is aware of her, you'll hear Ellie's
sprint breaths become increasingly feverish and fatigued.

These are her Stages.

Every tension has 3 stages of assets, varying in intensity.

When you let go of sprint, you'll hear the stages of her exhausted breath variations as it blends into her baseline breaths. This is one instance of Heart Rate that happens seamlessly throughout systemic gameplay.

In total, there are hundreds of breathing assets (single sound files of an exhale into an inhale) transitioning based on variables such as the tension, player health, enemy awareness, Heart Rate value, Vertigo scale (For Abby) and others.

The system is designed to ensure the assets never interrupt each other and always play out before game logic decides to pick a different type of breathing.

This implementation feels natural and makes you truly feel like you're controlling a living, breathing entity.

Heart Rate is even scripted based on story moments. I'm able to spike a character's Heart Rate to hear an audible 'come-down' in the breathing from a heightened state, like a jump scare.

In this video, I scripted a spike in Ellie's Heart Rate when she progresses
through this squeeze-through to showcase her fear. This blends beautifully with
the gameplay dialog which triggers after Ellie grabs the pipe!

We also override Murmuration with a custom loop for various story beats,
gameplay moments, or cinematic moments. This puts you in their shoes and helps
you empathize with their story by communicating how they're feeling.

Breath can be an excellent conveyor of emotion!

One note-worthy feature is the Open/Closed Mouth Stealth Breathing, Mike
Hourihan's (@m1keadelic) brilliant idea! If the enemy is within a certain
distance and in the player's view frustum, Ellie will transition to
closed-mouth breathing. This creates a tension that a player might not pick up
on, but affects them nonetheless. Sound has an interesting tendency to bypass
the brain and head straight to the heart.

Abby's vertigo was also integrated into this system. The vertigo game data that
affects the camera talks to the audio middleware to play different breathing if
Abby is looking over a ledge. The volume & intensity is affected by how much or
little you're looking over!

Injured breaths are integrated into the system as well. A flag is set when
there's an arrow embedded in the character or if player's health is low. That
enables my scripts to potentially play an injured variant integrated into the
the base systemic breathing variants.

All of the breathing for Player and Buddy characters were recorded with this
system in mind, giving us the most optimal source to be integrated into the
system.

Big thanks to all the main cast for entertaining our needs!


## Breathing System Thread Part 2

So now that there was a rich, detailed breathing system in place: it was time to marry the sound to the pixels!


Initially, we tried using the automatic lipsync that already existed in the
engine to drive the mouth from the sound signal. This was a fail, as it doesn't
play nice with 'efforts' or non-spoken dialog.

After this failure, we worked closely with animator Keith Paciello
(@keith_paciello) who was able to supply us with bespoke facial animations that
he hand-animated to the sound. There is a facial animation for every bucket of
breath variations for the player, from open-mouth stealth to exhausted stage 2
combat, and everything in between! Eli Omernick programmed these animations to
compress or expand, depending on the length of the sound file. This data is
sent in real-time from our audio middleware, so the facial animations are
completely reactive to the sound logic and retain the organic random behavior
designed in the middleware's sound script.

Abby Vertigo used this as well. Troy Slough (@Troy_Slough) & Karina V. were
able to animate a Abby's upper body for Abby's big, terrified breaths. The
upper body & facial animations were sync'd, driven by the sound. This brought
an organic feel to her human vulnerability.

All of these features married the sound to the pixels beautifully & produced
one of the most life-like gameplay facial animation I've ever seen in games.
Overall, this technology amalgamates to create an intense & visceral gameplay
addition to The Last of Us: Part II. It's similar to lighting, in that it's
hard to notice, but undoubtedly resonates with the player emotionally & creates
a palpable tension in the gameplay.

I must give credit where credit is due, as none of this would be possible without the brilliant minds that helped me assemble this system. Massive thanks to Eli Omernick who wrote all of the Heart Rate logic & Debug, Dog SFX System, and much, much more.

To Jonathan Lanier who added Murmuration to the dialog system as well as integrated (With the help of the TNT SCREAM team in San Mateo) the ability for our audio middleware to send data back to the runtime.

To animators Keith Paciello (@keith_paciello) for his relentless collaboration on creating the facial animations needed to make this look top-tier. Also to Karina Villanueva, Troy Slough (@Troy_Slough) and Ryan Broner for Abby Vertigo animation support.

To Mike Hourihan (@m1keadelic), Grayson Stone (@Anghil), Erik Schmall (@ErikSchmall) and Maged Khalil (@X144) for their critical dialog support and knowledge.

Lastly to Rob Krekel (@robkrekel), Neil Druckmann (@Neil_Druckmann) and Anthony Newman (@BadData_) for their leadership and feedback throughout these processes.

Can't forget where it all begins: the rec studio!

Thanks to all of our actors including Ashley Johnson (@TheVulcanSalute), Shannon Woodward (@shannonwoodward) & Laura Bailey (@LauraBaileyVO) who gave us a ton of breathing performances. They were not wasted! ♥️

I am very proud of the work we've done on this very special project.
Thanks for reading!
