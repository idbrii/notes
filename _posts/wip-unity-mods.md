---
layout: post
title: Making a moddable game in Unity
categories: [gamedev]

---

## Designing a moddable game

First, you need to figure out what aspects of the game you want moddable. There
are lots of ways modders could interact with your game:

* create levels
* create enemies that appear in user-created levels or your official levels
* create behaviours for objects (enemies, doors, buttons) that appear in user-created levels, your official levels
* create new features
* create new UI
* [load multiple mods at once and handle conflicts](https://www.reddit.com/r/gamedev/comments/oahr9i/how_moddable_games_handle_mods_compatibility/)
* override official content (models, textures, audio)

Find what makes sense for your game and limit it to that to reduce your scope.


## Making a moddable game in *Unity*

The rest of this post covers making a game where users build mods in the Unity
Editor. This is only one possible way: Modders could create everything within
your game (like Little Big Planet), you could [expose
Lua](https://www.moonsharp.org/) to modders, or you could load some json files
to change tuning values!


### Loading Assets

For our time-trial game, we focused on allowing modders to create levels.
Modders built their geometry in Unity (or imported meshes from elsewhere). We
had some MonoBehaviours they could chain together to make some basic logic (on
enter trigger, activate gameobject, or play animation, etc), but didn't support
actual code (even visual scripting). The game was essentially a time trial, so
it worked pretty well.

We exported a .unitypackage, uploaded it as a tool on Steam, and they could
import that package into Unity to get access to our scripts, gameplay, basic
assets, and also a library of assets from the game (doors, chairs, rocks, etc).
We had scripts to export their levels to asset bundles and upload to Steam
Workshop.

For levels to contain our MonoBehaviours/ScriptableObjects, export to
AssetBundle, and load back into game, we had to rewrite guids so the assembly
we shipped as a .unitypackage for modders to import would match the one running
in the game. We did it with some weird magic, but it seems that using asmdef's
is a better way. Put all of the logic that mods can access in an asmdef and
ship that in the mod sdk and it should load correctly in game. [Also asmdefs
can reduce your compile times](http://www.screaminggoose.com/blog/2019/2/4/how-i-cut-unity-compile-times-by-75),
so why not? Also, I think it's **easier to set that up now** instead of getting
it working later (it's pretty trivial to put all your code into one asmdef, but
harder to split it into two). I never had the chance to try it out on that
project.

### Loading Code

We never implemented it, but I looked into code support and these were the
three levels:

#### AppDomain sandbox

Modders write code that you compile to an assembly and you load it into another
AppDomain that doesn't have access to unsafe modules. I think Unity is/was
unable to *unload* AppDomains so you may have to restart the game to unload a
mod. Also, I'm not sure if you can allow access to Unity's AppDomain (the main
one) without also giving full file i/o access to the mod (no security/safety).

#### [Runtime compilation](https://somasim.com/blog/2017/12/27/tech-notes-unity-game-modding/)

modders bundle code as text and the game compiles it when loading the mod.
Allows you to sanitize the code and fail compilation if they use modules with
safety concerns. (Disable default type importing in the compiler and then
manually import the types you want. So you whitelist Rigidbody, but not
ReflectionImporter/File/etc.)

#### Full access

Mods are assemblies and you load them like your own.
[Harmony](https://harmony.pardeike.net/index.html) (originally created for
RimWorld) facilitates this, but (I think) modders can create nefarious mods and
break your game logic. Not sure there's much you can do to sandbox them.


[ref](https://www.reddit.com/r/gamedev/comments/rwa354/what_to_think_about_to_make_your_game_moddable/hrbgk3z/)
