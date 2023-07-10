---
layout: post
title: Implement different storefront APIs in your game
categories: [code, gamedev]

---

When you release a game on multiple storefronts, you often need to access their
APIs. Ideally, you do this without lots of special case code throughout your
game. You can do this by abstracting the retail platform's API behind a common
interface.

There are a few ways you can go about this abstraction in C-like languages:

1. make a base interface that your platforms inherit
1. make the platform-specific code the base class
1. make platform-specific APIs all use the same name

(1) is the most obvious:

    // retailplatform.h
    class RetailPlatform { ...api functions in here... };

    // steamplatform.h
    #ifdef STEAM
    class SteamPlatform : public RetailPlatform { ...api functions in here... }
    typedef SteamPlatform ActualPlatform;
    #endif

    // xboxplatform.h
    #ifdef XBOX
    class XboxPlatform : public RetailPlatform { ...api functions in here... }
    typedef XboxPlatform ActualPlatform;
    #endif
    ...

    RetailPlatform* ThePlatform = new ActualPlatform();



(2) is a bit weird, but because only one platform API will be defined at a
time, we can avoid duplicating function declarations in RetailPlatform and
using vtables:

    // steamplatform.h
    #ifdef STEAM
    class SteamPlatform { ...api functions in here... };
    #endif
    ...

    class RetailPlatform : public
    #ifdef STEAM
        SteamPlatform
    #elif XBOX
        XboxPlatform
    #else
        // a fallback with mostly empty functions
        StubPlatform
    #endif
    {}; // empty!

    RetailPlatform* ThePlatform = new RetailPlatform();

I like this method also because it's the easiest way to have a fallback else
case for a Stub. Having a Stub version of your abstraction is useful for
testing without connecting to your the storefront and as a starting point for
porting.

Also, with this method, we don't even need virtual functions. Since only one
implemention of each function will exist per build of the game, they're
unnecessary.


(3) feels a bit gross to me since the name isn't specific, but it avoids
needing a typedef or ifdef'd creation:

    // steamplatform.h
    #ifdef STEAM
    class RetailPlatform { ...api functions in here... };
    #endif
    // xboxplatform.h
    #ifdef XBOX
    class RetailPlatform { ...api functions in here... };
    #endif
    ...

    RetailPlatform* ThePlatform = new RetailPlatform();


You might look at this and say, "what about
[pimpl](https://en.cppreference.com/w/cpp/language/pimpl)?!" However, pimpl
would look like (1) except RetailPlatform contains a pointer to
RetailPlatformImpl which follows the (1) subclass pattern. I doubt you see an
improvement to compile times since you can put platform headers in your pch.
Hard to see benefits of adding that extra complexity.


[ref](https://www.reddit.com/r/gamedev/comments/1350bxg/how_do_you_manage_your_steam_versions_vs_nonsteam/jiigvym/?context=10000)
