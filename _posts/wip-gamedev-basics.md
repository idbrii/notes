## Enums

Enums are a common concept, but they can be a lot more flexible than just "a
list of possibilities". Frequently, you use them to index arrays to easily
select behaviour based on being in a specific state.

By enum I just mean defining a set of things and giving them incrementing
numerical values. I often do something like this:
```
-- https://github.com/rxi/lume
local lume = require('lume')

local actions_str = {
    'ATTACK',
    'DEFEND',
    'JUMP',
}
local actions_id = lume.invert(actions_str)

local id = actions_id.ATTACK
local as_str = actions_str[id] -- mostly for debug
local as_id = actions_id[as_str] -- but you can go back again
print("id= ", id, "as_str= ", as_str, "as_id= ", as_id)
```
Prints: `id=     1    as_str=     ATTACK    as_id=     1`
