---
layout: post
title: Game Localization
categories: [gamedev]

---

For context: I recently shipped a game translated into 14 languages and I was
the programmer who adapted our translation system from another game, updated
our game to use the system, and managed importing translations.

[This post is based on my comment on a translation company's post about
localization in
Unity](https://www.reddit.com/r/gamedev/comments/dd515a/game_localization_how_to_nail_game_localization/f2fbxzv/).
I've expanded my thoughts and made them generally applicable to anyone who
wants to start translating their game.


# Own your translation pipeline

My recommendation for programmers is to ensure you can programmatically process
your translations. You will likely do multiple rounds of translations. You will
accidentally send typos out to translation. You want your pipeline to reject
complete changes to strings (to get them re-translated) but accept minor
differences (to save you money and time). (See [Edit
distance](https://en.wikipedia.org/wiki/Edit_distance).) You may want to
translate text before it's ready to go out (ship language hotfixes for the game
while also creating DLC) so you'll it's helpful to support "unreleased"
translations -- only output text changes for the released content.

I'd recommend shipping your translations with your game as text files -- po or
csv are good formats. Using plain text allows players to create translation
mods without any work (they can modify an existing one) and will simplify
adding official translation mod support to your game. The po file format is
from the open source gettext project, so there's free tools for creating
translations like [poedit](https://poedit.net) and more expensive tools that
translation companies may use.

You can build your translation pipeline to collect several translation files
for one language, load up all their data, and then output the desired
translation. That way you won't re-translate something you already got
translated -- even if you moved it to a new id.

If you're targetting many languages, you may find your translators prefer csv
or po files, so it's a bonus if your pipeline can handle both. po files have
three standard fields: msgctxt, msgid, msgstr so you can output csv files with
those three columns. If you've done the work to support both of these formats,
it will be easier to add another -- but most likely translators can import one
of these format into their tools.


# Working with Translators

It's probably a good idea to talk to translation companies before you start
development on your localization tech, but beware of becoming locked-in to
their way of working. (We worked with four translation companies.)

Find out if they include Loc QA (where they play the game with their
translation to look for errors). If not (the cheaper ones probably won't even
look at your game), you'll need to figure out your own Loc QA solution.

Try to get native readers to look at the game and give feedback. Get
nonspeakers to run through looking for English. Your translators may
misunderstand something which leads to mistranslation. Also, be responsive to
fans reporting issues on bad translations.

You know your game super-well, so play in German. German words tend to be very
long (they form compound words like "freundschaftsbezeugung" for
"demonstrations of friendship") are really long so try to look for cut-off
text. For English speakers, German is often similar enough to English that you
can still play.


# Use hierarchical structure for your strings

On previous games I'd worked on, we had a flat list of all of strings:
IDS_001_GARY_SHOUTING_HI, IDS_002_CINDY_TELLING_GARY_TO_F_OFF. This seems
terribly unhelpful. You end up with weird numbering that seems useful, but when
someone adds IDS_181_GARY_PERV_WHISPERS it's unclear that 181 goes after 001
and fixing requires renumbering many unrelated lines.

Instead, I'd recommend a hierarchical structure (like nested classes or tables)
that you collapse into a long name (INTRO.HARASSMENT.001_GARY_SHOUTING_HI).
Translators won't see your hierarchy, but it will help force you to be
organized. I also avoid numbering unless ordering is really important (like
dialogue). The hierarchy will also maintain sensible order if you sort your
localization strings (which makes it easier to see changes in diffs).

Try to find a way to include context hints in your string structure. These
hints will help avoid many translator questions that can occupy your time and
slow down translator turnaround. Hints are especially important when using
string format functions that use numbered placeholders instead of names
(compare C# `string.Format("Pickup {0}", name)` to python's
`"Pickup {item_name}".format(item_name=name)`). The lazy approach would be to
tell translators not to translate anything ending with `_HINT` because those
are just hints for the preceeding string.


# Unique language issues

## Arabic

[Rami Ismail has a quick
starter](https://twitter.com/tha_rami/status/1178679339216494593) and [a
talk](https://www.youtube.com/watch?v=X1ynZm1wI18). If using Unity, look into
[arabic-support-unity](https://github.com/Konash/arabic-support-unity) for
Arabic letter connections and Right-to-Left.


## German, CJK, Arabic, etc

Test that your fonts display text properly in other languages. You may want to
build a fallback to [Noto](https://www.google.com/get/noto/).


# Support language mods

I've mentioned a few times how you can setup your pipeline to more easily
support language mods. If users just need to drop a strings.po file into the
right folder to support the mods, then you have a great start. The next step is
to build Steam Workshop support into your game to list subscribed mods and
write out the selected one as strings.po on startup (before strings are
loaded) and delete strings.po when selecting your default language.

Steam handles updating your mods and so long as you keep writing out the latest
on startup, the game will keep loading the latest mod. The only other thing you
need is an uploader. You can integrate the uploader into your game or find [an
open-source standalone Workshop uploader
](https://github.com/search?q=steam+uploader) and upload that as a Tool (instead
of a game) on Steamworks.


