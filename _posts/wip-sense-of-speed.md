---
layout: post
title: Conveying a Sense of Speed
categories: [gamedev]

---


# [How can a game give the player a sense of speed?](https://www.reddit.com/r/gamedev/comments/915uvx/how_can_a_game_give_the_player_a_sense_of_speed/)


> Speed lines! Like in anime shows. You don't have to go super obnoxious with it; just add a transparent layer over the image, with translucent lines (perhaps in white) that move in the opposite direction of the hawk's movement. You can also add some exaggerated motion blur to the background.


> Reference points. FOV. Speed lines. Those are the three easiest ways to make it appear players are moving quickly.

> Absolutely all of these and I'd try a tiny bit of camera wobble (not shake) to hint at instability.

> it's hard to get a sense of speed when you're not moving past something relatively close by. you might be able to accomplish this by putting "dust" particles in the air around the player

> Checkout [mario kart 8](https://youtu.be/OGm5mvZ_MGI) or wipeout for examples, narrow in the FOV circularly and then fisheye the edges of the screen or blur them. 

> The effect is simulating how eyes see things at high speeds. The peripheral vision is ultra compressed and simplified. Look straight out a window as a passenger and notice how your wetware processing is minimized on the low-detail blurry ground. Objects outside the center field of view are shrunken and appear to move much faster than the objects of central focus. One way to fake this is to hard code sprites that look like they are laid inside a bowl (convex transformation) and swap them in when diving/boosting, cover up the ugly bits with speed lines. Hope these [unity question about fisheye](https://answers.unity.com/questions/62645/how-to-get-fisheye-from-field-of-view.html)  unity question about [convex lens](https://answers.unity.com/questions/148094/how-to-do-concave-and-convex-mirror-effects-in-uni.html) help.

> Sound will also help alot, could add some whistling noises or wind buffeting (tea kettle about to boil). 


> Squash and stretch the hawk relative to its speed.
> If you haven't yet try adding sound effects.
> If it's a 3D game then increasing the FOV and motion blur can help.


/u/TankorSmash
> [Check this out, at about 0:29](https://youtu.be/Ds7AdmHKXi8?t=29)

> * Subtle screen shake
> * Things moving by you quickly (motes of dust, clouds)
> * wind rushing passed your ears
> * fov changing
> * distance from camera getting close
> * controller vibration
> * motion blur

/u/Mohagged
> Some (general) things that come to mind:

> * high FOV; especially something like a quick FOV increase when boosting speed (example: [Star Wars Speeder](https://www.youtube.com/watch?v=1zEbkT8Sw6c)); or when it comes to contrast: a small FOV when going slow, and a high one when going fast
> * (on third person) pawn distance to camera (example: [Star Wars Speeder AGAIN!](https://youtu.be/nTjufSjRTpg?t=133))
> * pawn size in relation to other objects: scaling objects down (even if the speed stays relatively the same) you'll feel faster relative to these objects -- on the flip side: scaling your pawn (squishing)
> * passing objects: passing by more stuff faster (example: several different styles of "dust particles" like [Star Citizen](https://youtu.be/hMfvny2NRHs?t=1154)) -- also includes objects (seemingly) moving in your direction
> * land marks (the help with the above mentioned) like towers, mountains, trees, cloud formations
> * camera height: putting your camera closer to the ground makes it seem faster (for instance in a car racing game) (for third person this might relate to the camera "underneath" the pawn, not sure though)
> * details (for instance in textures): they add to noise
> * camera shake (headache inducing for many players)
> * camera distortion, radial sharpness/blur (tunnel vision) and motion lines (example: [Mirror's Edge](https://www.youtube.com/watch?v=6D1KzmRFNPs))
> * "horizontal lines" (relative to your movement direction) seem faster than vertical lines (example: curb lines in racing sports)
> * controller vibration
> * time dilation for everything but the pawn
> * camera "color explosion" effect when boosting (example: a sudden increase in saturation, bloom, and other effects)
> * pawn [trails](https://youtu.be/3SPKx7F8rVo?t=94) and [ghosting effects](https://youtu.be/lGtTwNz-Wq0?t=71) -- also jet trails and sonic boom effect (when boosting)
> * audio noise (turbulent headwind to the point of going "silent") and "urgency/action" music/sfx (clock ticking, heart beat, ...)
> * if you have a crosshair/HUD: change size (slow: big/close up -- fast: tiny/distant)
> * animating the hawk's feathers to move violently when going fast
> * animation skipping (often done in melee combat games, example: [Last of Us](https://youtu.be/yH5MgEbBOps?t=3981))
> * [Warframe: Volt](https://youtu.be/G0cWeSp6Y7c?t=80)
> * fake information (example: speed-o-meter which shows high numbers when going fast, low numbers when going slow -- even if they are fake; example2: some old school car games had multiple cars with different stats but they were all the same)

> Radial blur is a quick and easy way to achieve a sense of speed. [It even works in still images!](https://i.ytimg.com/vi/Y16jJfIxcHc/maxresdefault.jpg)


[How Can I Convey a Sense of Moderate Speed to the Player?](https://www.reddit.com/r/gamedev/comments/75nv1f/how_can_i_convey_a_sense_of_moderate_speed_to_the/)

/u/Ultraviolet_Elite
Slower turn radius while moving quickly b/c of fighting your momentum
Tweak acceleration so that getting up to high speeds feels significant

/u/PiLLe1974
Then there is sound: idle speed, cruise speed, boost, ... and general "whooshing" sounds like whatever you imagine when you hear the wind.

/u/BeayemX
At the moment all suggestions are about your surroundings, but what about things that belong to the player.

You could use things like capes, flags or other cloths that flap stronger the faster you go.

You could use things like antennas that bend stronger the faster you go.

And you could spawn particles from the player object itself like exhaustion fumes. Maybe with a short life span. That way the player would see that they are going fast when there is a long trail.



https://youtu.be/3XXw-2EPlL0?t=1680
# [The Physics of Fun: The Vehicles of Saints Row](https://www.youtube.com/watch?v=3XXw-2EPlL0)
