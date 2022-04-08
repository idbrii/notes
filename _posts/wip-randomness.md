*These are my comments based on some lib someone wrote. Can I turn this into
an interesting discussion of randomness?*

It's weird that roll.lua calls math.randomseed on every roll. Normally, you see
a random number generator once and then call it many times. The seed determines
the sequence of numbers, so they're actually introducing the problem that
they're solving by seeding with time + weirdness. If they seeded once, then
they could roll multiple times per second and still get good random rolls.

Given that, I'd say don't use it and use love's random instead. You can write a
function to roll dice without all that extra string parsing:
```
function roll(count, max)
    local sum = 0
    for i=1,count do
        sum = sum + love.math.random(1, max)
    end
    return sum
end

-- 3d6
print(roll(3, 6))

-- 2d20
print(roll(2, 20))
```

I believe love's random is supposed to produce better random numbers than lua's
random. More importantly, it's platform independent and you can create rng
objects so you can use it as I describe above for PC vs Linux multiplayer.


