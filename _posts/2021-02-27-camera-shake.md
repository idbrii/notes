---
layout: post
title: Be continuous -- Don't use random in your screenshake
categories: [gamedev, code]

---


Screenshake should move the camera around smoothly but unpredictably.

![move the camera around smoothly but unpredictably](https://www.theappguruz.com/app/uploads/2018/06/starting-gif-1.gif)

It shouldn't jitter the camera around in a way that makes it hard to follow
what's on screen. That's why you should use continuous oscillating functions to
produce your shake instead of random values. I also think it's useful to make a
directional shake to help connect the shaking to the thing that caused it --
shake the camera away from an explosion.

Since it seems most camera shake tutorials show you how to use random shake,
here's one for how to use continuous functions to produce shake. I'm using sine
and perlin noise because they're easily accessible, but you could use any
oscillating continuous function.

On to the shake:

```cs
// Our inputs:
Transform _Target;
float _Seed = 0f;
float _Speed = 20f;
float _MaxMagnitude = 0.3f;
float _NoiseMagnitude = 0.3f;
Vector2 _Direction = Vector2.right;

// Use sine to get a value that oscillates with time. This makes our camera
// move back and forth. Scale time with _Speed to shrink or grow the period of
// the oscillation which makes the shake faster or slower.
// Since shakes are tied to time, you may see multiple objects shaking in sync.
// The _Seed value allows you to offset shakes. You could set it to a random
// value in Start.
var sin = Mathf.Sin(_Speed * (_Seed + Time.time));
// Shake along a direction, but use Perlin noise to get an offset. Scale
// the noise (which is in [-0.5,0.5]) to adjust the amount of deviation
// from our direction.
var direction = _Direction + Get2DNoise(_Seed) * _NoiseMagnitude;
// Normalize the result (limit vector length to 1) to ensure we're never
// more than _MaxMagnitude away from neutral.
direction.Normalize();
// Multiply our bits together to find our position this frame. Since we're
// using two continuous functions (sine and perlin), we won't be far off
// from where we were last frame.
// Additionally, we have a fade value so we can reduce the shake strength
// over time.
_Target.localPosition = direction * sin * _MaxMagnitude * _FadeOut;
```

You can [see how it looks](https://imgur.com/a/9h3c6W5) to shake the camera or an object (Using [Easing.CircIn](https://github.com/idbrii/cs-tween/blob/bb05ab0a8526a11d3c58a757ec406fbdf9dc2513/Easing.cs#L186-L189) instead of linear for FireOnce):

<p>
  <video width="75%" controls>
    <source src="https://i.imgur.com/GPRyIaE.mp4" type="video/mp4">
Your browser does not support the video tag. Try <a href="https://imgur.com/a/9h3c6W5">viewing on imgur instead</a>.
  </video>
</p>

<p>
  <video width="75%" controls>
    <source src="https://i.imgur.com/1QD7G2B.mp4" type="video/mp4">
  </video>
</p>

And compare it against a random shake (`_Target.localPosition = UnityEngine.Random.insideUnitCircle * _MaxMagnitude * _FadeOut`):

<p>
  <video width="75%" controls>
    <source src="https://i.imgur.com/RjPqxgY.mp4" type="video/mp4">
  </video>
</p>




The [full Unity implementation](https://gist.github.com/idbrii/6249ebccb08dc7ae5b75603cf261c0f5):

{% gist idbrii/6249ebccb08dc7ae5b75603cf261c0f5 %}

Be sure to provide a user option to disable shakes! They make [some people
nauseous](http://gameaccessibilityguidelines.com/avoid-or-provide-option-to-disable-any-difference-between-controller-movement-and-camera-movement-such-as-weaponwalk-bobbing-or-mouse-smoothing/).
You could easily scale the `_MaxMagnitude` to shrink down all of your shakes if
you want to provide multiple levels of shake instead of only zero shake.

----

If you're interested in more, watch [Juicing Your Cameras With
Math](https://www.youtube.com/watch?v=tu-Qe66AvtY). Squirrel is a great speaker
and he talks more about using noise for your shake, goes into rotational shake,
and describes a better way to think about how much shake to apply (trauma).
There's also other camera techniques in the talk (smooth motion, framing,
split-screen).
