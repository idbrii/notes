---
layout: post
title: Kirby's 2D Gameplay Ideas in a 3D World (Talk)
categories: [gamedev, techtalk]

---

# [Kirby and the Forgotten Land: The designers discuss how they brought traditional Kirby gameplay into a 3D world](https://www.youtube.com/watch?v=cWdt07ncRxU)

[Posted on r/TheMakingOfGames](https://www.reddit.com/r/TheMakingOfGames/comments/12pnwol/kirby_and_the_forgotten_land_the_designers/)

Kamiyama's section was great. Here's some major points.

A main point of the talk was designing for anyone. Since they're making a 3D game, many issues arise from how to make things intuitive in the camera angles and movement freedom that 3D brings.

## 12:01 Aiming projectiles
Kirby is a sphere, so it's hard to tell which direction he's facing. Solution: sophisticated homing.

* (13:20) Score enemies based on facing direction, distance, importance/priority.
* Distance is directional so it doesn't catch airborne enemies when Kirby is grounded.
* Airborne aiming is hard, so homing is stronger when flying.
* Homing works in two stages. In the first frame, projectile facing snaps to target. After that, homing is limited. Goal is to make players feel like they're successfully aiming instead of homing.

## 15:40 Unique hit detection system
It's difficult to get a sense of distances in 3D. If you're aiming in the ground plane but viewing along that plane, it's hard to tell when you're shooting in front or behind objects. Solution: Apply hits based on whether it looks like they hit.

* (16:53) Extend hit direction in the depth direction. Not just larger hitbox, but it's extended in the depth direction using the camera position.

## 19:01 3D cameras are challenging for beginners
It's hard to control both character and camera at the same time. It's hard to be aware of your surroundings in 3D. Solution: Level designers configure cameras instead of player control.

* Apply 2d level design knowledge.
* Shift camera to include relevant objects (hazards, paths).

## 20:38 Hovering out of bounds
Kirby can fly so players will want to explore out of bounds. Solution: hint boundaries with camera.

* (22:35) Camera object hits walls that prevent its movement. So when you walk right and hit a barrier, the barrier stays on the right side of the screen and Kirby can't push the camera further right to see what's there (there's nothing there).
* Radially project from desired camera focus point to find camera boundaries and smoothly adjust the actual focal point. (Gotta watch because the explanation is not very technical.)


