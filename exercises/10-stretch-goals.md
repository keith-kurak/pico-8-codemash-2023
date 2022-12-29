# Strech Goals/ Bonus Stuff
Some extra things to do if we have extra time

## Add power pizzas
Add a collectable that increases your score!

First, make a pizza/ other collectable sprite

On tab 2, add a function that returns a height to place a pizza, and -1 if none should be placed. This example only places pizzas when the terrain is at its highest or lowest, but you can do whatever.

```lua
function pizza_pos(flr_hgt)
 -- copy from gen_flr_hgt
 local max_flr_hgt = flr(1 + 3 + level * 0.34)
 -- randomly decide to show a pizza if the terrain permits
 local could_have_pizza = rnd(100) > (88-(level*2))
 -- add pizza at highest peaks
 if (flr_hgt == max_flr_hgt and could_have_pizza) then
  return flr_hgt + 4
 end
 -- add pizza at lowest valleys
 if flr_hgt == 1 and could_have_pizza then
  return flr_hgt + 1
 end
 return -1
end
```

In `gen_map()`, add this bit for each line to potentially add a pizza:
```lua
local pizza_hgt = pizza_pos(flr_hgt)
if pizza_hgt != -1 then
 mset(i,15-pizza_hgt,PIZZA_SPRITE)
end
```

Finally, check for pizza collision in `_update60()`:
```lua
if hit(p.x,p.y,7,7,4,true) then
 -- 20 points, since points = dist / 8
 p.dist = p.dist + 160
end
```
