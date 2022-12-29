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

## Double jump
Notice that `player.jump` is an integer? You can make double-jumping possible just by changing one line of code in `_update60()`.

The real trick here is that, if all you do is change that one line, the second jump will often be way too high, so figure out what kind of acceleration is appropriate for the two jumps, as well.

Finally, note how that affects the difficulty of the game. Should you tweak your level generation?

## Really basic enhanced level generation ideas
This level generation is way too simple. It's just random stuff that doesn't consider its surroundings. One of the simplest things to do is actually consider your surroundings.

### Ideas
#### Set a limit for consecutive lava tiles
Each time you place a lava, increment a counter. Don't let it go over a certain amount, possibly adjustable based on your current level.
#### Set a limit for incline/ decline
If n-1 is 6 tiles high, don't let the next one go any lower than 3, or vice versa.
#### Favor plateaus
Instead of randomly picking the next height, roll the dice twice: once to see if the height will be the same, and, if it's not, again to see what the new height will be.
#### Floating blocks
Randomly float a block based on certain conditions, e.g.,
- 10% chance of a floating block over lava
- 10% chance of a floating block 3 tiles above the highest floor level
