---
layout: post
title: Subtitle Technology
categories: [gamedev]

---

TODO: Right now, this is a jumble of notes on implementing subtitles.

I'd recommend reading
[Netflix Timed Text Style Guide](https://partnerhelp.netflixstudios.com/hc/en-us/articles/215758617-Timed-Text-Style-Guide-General-Requirements) and
[BBC guide on Timing](http://bbc.github.io/subtitle-guidelines/#Timing) to understand how to craft good subtitles.

Two of the rules I enforce via code derived from those articles:

* Enforce a blank between titles to help viewers notice the text change (Frame Gap).
  * https://partnerhelp.netflixstudios.com/hc/en-us/articles/215758617-Timed-Text-Style-Guide-General-Requirements
* Enforce display duration that ensures reasonable WPM reading speed expected of the player. (I use 330 ms/word.)
  * http://bbc.github.io/subtitle-guidelines/#Timing

There are many other rules that are hard to automate, but you can follow them when you write your subtitles. I setup my subtitles for English and rely on translators to apply appropriate rules and redistribute words across lines as appropriate.

I [wrote about my localization system before](notes/games-localization/), but I'll recap the basics:

I use hierarchical string ids in gettext's po format: 

    #. STRINGS.DIALOGUE.SCENE_1_CONFLICT_BEGINS.LINE_000_SPEAKER_GENERIC
    msgctxt "STRINGS.DIALOGUE.SCENE_1_CONFLICT_BEGINS.LINE_000_SPEAKER_GENERIC"
    msgid "Our story begins."
    msgstr ""

    #. STRINGS.DIALOGUE.SCENE_1_CONFLICT_BEGINS.LINE_001_SPEAKER_SULWE
    msgctxt "STRINGS.DIALOGUE.SCENE_1_CONFLICT_BEGINS.LINE_001_SPEAKER_SULWE"
    msgid "Nothing will stop me"
    msgstr ""

    #. STRINGS.DIALOGUE.SCENE_1_CONFLICT_BEGINS.LINE_002_SPEAKER_SULWE
    msgctxt "STRINGS.DIALOGUE.SCENE_1_CONFLICT_BEGINS.LINE_002_SPEAKER_SULWE"
    msgid "from rescuing my people."
    msgstr ""

Both fan and official translators provide their own po file with msgstr filled in. I have a system to load the po into a dictionary with the msgctxt as a key and msgstr as a value (with the msgid as a fallback for empty msgstr).

Notice that each line has a clear scene for context, line number for ordering, and speaker name.

For dialogue, I have another system with a ScriptableObject that aggregates the strings for a scene with their timing. (Using hierarchical ids makes it easier to automatically pull in all strings for a scene.) It can also import/export to [SRT format](https://github.com/AlexPoint/SubtitlesParser) because I use Youtube to help auto-set timings for my subtitles ([with a SRB->SRT converter](https://gidsgoldberg.com/sbv_docs_converter.shtml)). I use coroutines to wait for fmod to hit the timestamp and then display the next subtitle. I never WaitForSeconds longer than 10% of the time until the next line in case fmod skips around in the timeline -- this happens to me when other stuff is loading.

You need to think about that before sending strings off to translation because you want to cut up sentences into fragments that you display to avoid spoiling jokes and presenting too much text at once. 
