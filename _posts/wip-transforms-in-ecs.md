Write about writing a nested transform system in an ecs.
Other alternatives
https://bitbucket.org/itraykov/love.scene/src/master/
https://github.com/flatgames/amour/blob/master/amour/node.lua

Rough overview of the code:

```lua
function transform.create_component(x, y)
    local data = {}
    -- define transform properties.
    data.pos = Vec2(x,y)
    data.rot = {
        angle = 0,
        pivot = Vec2(0,0),
    }
    data.scale = Vec2(1)
    data.skew = Vec2(0)

    -- Our transform that we'll flatten each tick.
    data.flattened = love.math.newTransform()
    function data:_write_to_transform(target)
        target:setTransformation(
            self.pos.x, self.pos.y,
            self.rot.angle,
            self.scale.x, self.scale.y,
            self.rot.pivot.x, self.rot.pivot.y,
            self.skew.x, self.skew.y)
    end
    data:_write_to_transform(data.flattened)

    function data:set_parent(parent_ent)
        -- When parented, all of our values are relative to our parent.
        parent_ent.transform.t_children = parent_ent.transform.t_children or {}
        if self.parent_ent then
            lume.remove(parent_ent.transform.t_children, self)
        end
        if parent_ent == nil then
            -- unparenting
            return
        end
        assert(type(parent_ent) == 'table', "Must set valid parent. Should be an entity.")
        assert(type(parent_ent.transform) == 'table', "Must set valid parent. Should be an entity with a transform component.")
        assert(parent_ent.transform ~= data, "An entity cannot be its own parent.")
        self.parent_ent = parent_ent
        table.insert(parent_ent.transform.t_children, self)
    end
    return data
end
```

```lua
-- A temp transform for our calculations.
local scratch_t = love.math.newTransform()
local function propagate(t)
    if not t.t_children then
        return
    end
    for i,child in ipairs(t.t_children) do
        child:_write_to_transform(scratch_t)
        child.flattened:reset()
        child.flattened:apply(t.flattened)
        -- Position children relative to our pivot so multiple stacked objects
        -- rotate on the same point. Not sure if this will later make using
        -- pivots more difficult, but the alternative is requiring children to
        -- add their parent's offset.
        child.flattened:translate(t.rot.pivot.x, t.rot.pivot.y)
        child.flattened:apply(scratch_t)
    end
    for i,child in ipairs(t.t_children) do
        propagate(child)
    end
end

function transform.update(self, dt)
    for i,ent in ipairs(self.entities) do
        local t = ent.transform
        if not t.parent_ent then
            -- Only directly _write_to_transform for root transforms. Children
            -- will be updated in propagate.
            t:_write_to_transform(t.flattened)
            propagate(t)
        end
    end
end
```



```lua
function visual.preupdate(self, dt)
    for key,ent in pairs(self.entities) do
        local v = ent.visual
        -- Push our offset as our rotation pivot so we'll rotate around it.
        ent.transform.rot.pivot.x = -v.offset.x
        ent.transform.rot.pivot.y = -v.offset.y
    end
end

function visual.draw(self)
    for key,ent in pairs(self.entities) do
        local v = ent.visual
        if v.enabled then
            local t = ent.transform

            love.graphics.push()
            do
                love.graphics.applyTransform(t.flattened)
                love.graphics.setColor(v.color)
                v:draw(0,0)

                -- Debug: Draw bounding box
                --~ love.graphics.setColor(0.8, 0.8, 0.8, 1)
                --~ love.graphics.rectangle('line', v.bounds.x, v.bounds.y, v.bounds.width, v.bounds.height)

                love.graphics.setColor(1, 1, 1, 1)
            end
            love.graphics.pop()
        end
    end
end
```
