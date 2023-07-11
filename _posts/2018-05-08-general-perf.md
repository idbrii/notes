---
layout: post
title: Notes on CPU Performance
categories: [code, gamedev]

---

Some clippings of advice I've given to others on good places to learn about CPU
performance. Or reminders for myself.


## DOP

Watch [CppCon 2014: Mike Acton "Data-Oriented Design and C++"](https://www.youtube.com/watch?v=rX0ItVEVjHc) ([slides](https://neil3d.github.io/assets/img/ecs/DOD-Cpp.pdf)), because it explains the motivation for Data-Oriented Processing. The speaker, Mike Acton, was engine director at Insomniac and later a major driver of [ECS](https://en.wikipedia.org/wiki/Entity_component_system)/[DOTS](https://unity.com/dots) at Unity. You can also read [his code review of Ogre](https://www.bounceapp.com/116294) and the Ogre team's [very reasonable response](http://www.yosoygames.com.ar/wp/2013/11/on-mike-actons-review-of-ogrenode-cpp/).

See also Andreas Fredriksson's [Optimizable Code](https://deplinenoise.wordpress.com/2013/12/28/optimizable-code/) article that Mike Acton discusses in that DOD talk.

If all of those are too long, then his [Typical C++ Bullshit](https://macton.smugmug.com/Other/2008-07-15-by-Eye-Fi/n-xmKDH/i-BrHWXdJ) (now offline [even on wayback](http://web.archive.org/web/20130919114604/http://macton.smugmug.com/gallery/8936708_T6zQX#!p=4&n=15)) is a quick primer using a specific example for how iterating objects is bad.

## SOA vs AOS

Using structures of arrays is really about cache hits/misses.

Imagine you're coding a football game. You could store your team data in two manners:

```cs
// An array of structures:
struct Player {
    Vector3 position;
    Quat rotation;
    Vector3 velocity;
    string name;
    Country birth_country;
    Country team_country;
    int player_number;
    // ...
};
var team = new Player[MAX_PLAYERS];

// A structure of arrays:
struct Players {
    Vector3[] position;
    Quat[] rotation;
    Vector3[] velocity;
    string[] name;
    Country[] birth_country;
    Country[] team_country;
    int[] player_number;
    // ...
}
var team = new Players();
```

Imagine you want to tick their movement. You multiply their velocity by delta time and add it to their position. When you access their data (position, velocity), you will load that data into your cache. Cache prefetching will load in surrounding data too.

With the array of structures, you process one structure (player) at a time. But since we don't care about the surrounding data, we blew our cache and the next player will be a cache miss.

With the structure of arrays, you process two elements of arrays (relevant data) at a time. This time, the surrounding data is the next player (and the one after that), we'll get cache hits.

[ref](https://www.reddit.com/r/gamedev/comments/87ikb9/ecs_newb_seeking_clarity/dwdhpxa/)
