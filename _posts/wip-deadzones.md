---
layout: post
title: Thumbstick Dead Zones
categories: [gamedev, code]

---

# Deadzones in Other Games
I found [this gallery of deadzones](https://imgur.com/a/xSfcP) today. Looks like [EternalDahaka is the creator](https://www.youtube.com/watch?v=ljvOEGpcYVM) and has [more data here](https://mobile.twitter.com/EDahaka/status/1503934512492957699).

It's a huge gallery with no text so navigating it is awkward, but it was
interesting to see some alternatives to axial or radial deadzones. Anarchy
Reigns has [an unusual shape](https://i.imgur.com/2OCluE6.png) and I wonder if
you can feel the difference when playing. Also interesting to see how games in
a series changed their deadzone.

Side note: Hall effect sensors or any other kinds of "perfect" hardware doesn't
necessarily solve gameplay reasons for having deadzones. Sometimes you want
pure horizontal/vertical movement to be easier to execute so you use an axial
deadzone.

For more about implementing deadzones, [Doing Thumbstick Dead Zones
Right](http://joshsutphin.com/2013/04/12/doing-thumbstick-dead-zones-right.html)
(looks like this is the new home of the [classic third helix
article](https://web.archive.org/web/20190129113357/http://www.third-helix.com/2013/04/12/doing-thumbstick-dead-zones-right.html)
that was also [on GameDeveloper](https://www.gamedeveloper.com/disciplines/doing-thumbstick-dead-zones-right)
but missing images). It's a good overview and shows code and value visualizations for different kinds of deadzones.

It's so great, that I'm going to paraphrase its final two sections:

## The Right Way — Scaled Radial Dead Zone
Clip and rescale the input vector into the non-dead zone space to get a smooth transition out of your deadzone:
```
float deadzone = 0.25f;
Vector2 stickInput = new Vector2(Input.GetAxis("Horizontal"), Input.GetAxis("Vertical"));
if (stickInput.magnitude < deadzone)
    stickInput = Vector2.zero;
else
    stickInput = stickInput.normalized * ((stickInput.magnitude - deadzone) / (1 - deadzone));
```
If you visualize the values, there’s no longer a visible edge: as you push the
stick away from the center, the gradient value changes smoothly while the dead
zone is still preserved.

## Dead Zones For Fun and Profit
I called it the “right” way but that doesn’t mean you’ll never use any other
method, ever. The most important thing is to use the method that makes sense
for your particular project. Here are a few scenarios:

* Tile-based (4-way) movement: The Axial Dead Zone actually works well here since it snaps analog input to the only four input vectors that are actually relevant.
* Twin-stick shooter: In these games the magnitude of input rarely matters — all you care about is direction — so the simple Radial Dead Zone should be perfectly suitable here.
* Super-polished FPS: Sometimes you need to sweep your aim through a line, and keep the crosshair on or close to the line. In this case you might want to blend a Scaled Radial with a modified Axial Dead Zone, such that the stronger your input in one axis, the larger the dead zone gets for the other axis. (At LightBox we called this the “bowtie” because the dead zone diagram looks like… a bowtie. I’ll leave the implementation of this one as an exercise for the reader!)

Now go forth and implement your dead zones properly! It’s easy, and your players will appreciate it. :)

Go read the full [Doing Thumbstick Dead Zones Right](http://joshsutphin.com/2013/04/12/doing-thumbstick-dead-zones-right.html)
article to understand all the different deadzone types.


## More Reading

Minimuino also has [Understanding thumbstick
deadzones](https://github.com/Minimuino/thumbstick-deadzones) which is an
extension of Sutphin's article. It goes deeper with a hybrid deadzone.

Jibb Smart's [2D Steering - How to Make Games Steer
Better](https://cohost.org/jibbsmart/post/407479-2d-steering-how-to) is a
completely different take on deadzones: use the outer extents of the thumbstick
for 1d input so you don't need a deadzone. Since the stick must be deflected to
count as steering input, you're completely side stepping the deadzone. Not sure
how natural this would feel to most players.


[ref](https://www.reddit.com/r/gamedev/comments/zdptoj/images_of_deadzones_for_many_games/)
