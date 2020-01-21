---
layout: post
title: Understanding Quaternions
categories: [math, gamedev]

---

WORK IN PROGRESS. I'm still working on writing this one.


Collecting some notes to help build an understanding of quaternions for use in gamedev.


Someone named LaurieCheers [wrote the most useful descriptions of quaternions that I've seen](https://www.reddit.com/r/gamedev/comments/9s1uj6/amazing_explanation_of_quaternions_for_3d_rotation/e8lq5ae/):

> To put it in terms game developers are probably more familiar with: this means that you can use quaternions to compose rotations, in much the same way that you can use vectors to compose translations.
> 
> When you multiply two quaternions `q1*q2`, you're composing those rotations. (The result is exactly like making a GameObject in Unity with rotation `q1` and giving it a child object with localRotation `q2`.)
> 
> The equivalent for vectors is addition - if you have a point `p and` an offset `o,` the final position is `p+o`.
> 
> We can also decompose them - if you know two points `p1` and `p2` and you want to know the offset between them, you'd do `p2-p1`. Or to put it another way, `p2+(-p1)`.
> 
> Similarly if you have two quaternions `q1` and `q2`, and you want to know the local rotation between them, you'd calculate `(inverse(q1))*q2`. Inverse is the equivalent of negating a vector, multiplication is the equivalent of adding vectors.


That was a reply to a longer comment on [useful properties of quaternions](https://www.reddit.com/r/gamedev/comments/9s1uj6/amazing_explanation_of_quaternions_for_3d_rotation/) explaining some quick fundamentals and then how you can apply them. If you can make it through

A quaternion represents a rotation. 

Quaternions are often thought of as a rotation around an axis and math
libraries can create a quaternion from an axis and angle.

A quaternion is stored as a Vector4 (whereas translation vectors are a Vector2
in 2D and Vector3 in 3D). The four components are usually labeled w, x, y, z
and they correspond to four dimensional complex numbers where w is the real
number component, and x,y,z are i,j,k.

A unit vector can be thought of as a quaternion with the real component equal
to zero (w=0). This model is how we implement vector-quaternion multiplication.

Multiplying a vector `v` by quaternion `q` results in a vector that was rotated
by q.

> So if q described a flat rotation 180 degrees (in the xz-plane) and v
> described looking forward, then `v * q` describes looking backward.

You can multiply quaternions together. This is not commutative (the order
matters). The result is like applying one rotation and then the other -- but the order is reversed. `q * r` is equivalent to rotating by `r` and then `q`.

> If `r` described a rotation up 45 degrees, then `v * (r * q)` describes looking
> backward and up 45 degrees.

TODO: verify that's correct

However, multiplying many quaternions together, we eventually need to normalize
the quaternion to prevent floating point errors from accumulating and making it
no longer a unit quaternion. Normalizing a quaternion is just like normalizing
any other Vector4.

Exponents multip
> 8. Rotations may be divided or multiplied by quaternion powers:
> 
>         if q represents a rotation of x around some axis, then q^2 represents a rotation of 2x,
>         and q^0.5 represents a rotation of 0.5x around that axis.
> 
>                 q^x = |q|^(x) * ( cos(x * theta) + n1 * sin(x * theta) + n2 * sin(x * theta) + n3 * sin(x * theta)).
> 
>         Note that if x is -1, then this reduces to the inversion.
> 
> 9. Changing the coordinate system of a quaternion (this is super useful):
> 
>         Suppose the following:
> 
>         1. You have some canonical defined orientation that is consired "zero rotation", ie.
>            "facing down the positive x axis with "up" being up the postitive y axis."
> 
>         2. You have a spaceship whose current orientation is defined as a quaternion representing
>            the rotation from this zero rotation, call it r.
> 
>         3. You have a rotation which you want to apply to the spaceship in its orientation.  Let's
>            say you want to yaw 5 degrees left.  So you construct a quaternion, q, to represent a 5
>            degrees left yaw from "zero rotation", 5 degrees around the y axis:
> 
>        q = cos(0.5 * 5 * pi / 180.0) + 1.0 * sin(0.5 * 5 * pi / 180.0) * j; /* i and k components are 0 because we want 5 deg about y axis */
> 
>         To apply q to r, to get a new orientation quaternion, n, we can do something analogous to rotating a
>         vector by q.
> 
>         n = rqr^(-1) = r(q0 + q)r^(-1)
>           = rq0r^(-1) + rqr^(-1)
>           = q0rr^(-1) + rqr^(-1)
>           = q0 + rqr^(-1)
> 
>   Now you have your 5 degrees left rotation quaternion n in the spaceship's coordinate system and you can apply it to your spaceship's current orientation to turn it 5 degrees left (as in item 7, above).
> 
> Main properties of quaternions:
> 
>         1. pq is not equal to qp
>         2. p(q + r) = pq + pr
>         3. (sp)q = p(sq) = s(pq)
>         4. q^(-1) = (q0 - q) / |q|^2
>         5. p(qr) = (pq)r
>         6. qvq^(-1) rotates a vector by q
>         7. pq composes two rotations
>         8. q^x does rotation q but x times (ie q^3 does rotation q 3 times)
>         9. rqr-1 rotates the rotation axis of

Some other resources from most to least helpful (for me):

* [quaternions.online](https://quaternions.online/) -- enter quaternion values and see them represented in 3D and as Euler angles. Helpful to explore a bit and maybe for debugging (you could copypaste values from Unity inspector to see what the rotation means).
* [Visualizing quaternions -- An explorable video series](https://eater.net/quaternions) -- Trying to explain quaternions by visualizing them. Technically impressive because it's a series of interactive videos, but I don't think this helped me use them.
* [Math for Game Programmers: Working with 3D Rotations](https://www.gdcvault.com/play/1020043/Math-for-Game-Programmers-Working) -- walking through examples of rotations and then how you'd apply them with quaternions.
* [Basic operations of quaternions](http://www.allenchou.net/2014/04/game-math-quaternion-basics/)
* [Let's remove Quaternions from every 3D Engine](https://marctenbosch.com/quaternions/) -- a pitch for an alternative (more intuitive) representation of rotations. I think this is very mathy and not very helpful when working in an engine using quaternions.
