# Changing the environment
## Goal
Further motivate the player to proceed forward by creating level progression
## Tasks
1. Update a "level" value each time the map regenerates
2. Change the backdrop color when the level changes
3. Increase the difficulty as the level increases
4. Add a basic "title screen" state
## Useful info
### New API's

## How to code it
### 1. Update the level
In `_init()`, initialize a level variable:
```lua
level = 1
```

Draw the level with the rest of the HUD in `_draw()`:
```lua
print("lvl",92,1,10)
print(level,112,1,7)
```

Update the level when calling `gen_map()` in `_update60()`:
```lua
level = level + 1
```

### 2. Change the background when the level updates
Pick three colors that don't clash with your existing sprites as backdrops, and set those at the top of tab 0:
```lua
backdrops = {
 12,13,0
}
```

In `_draw()`, before drawing the map, draw the backdrop:
```lua
rectfill(0,0,128,128,backdrops[level % 3])
```

### 3. Add a not started/ title state
Add a variable at the top of tab 0 to indicate that the game hasn't started:
```lua
not_started = true
```

Update the game over code in `_update60()` to account for this state:
```lua
 if p.gameover or not_started then
  if (btnp(🅾️)) then
   not_started = false
   _init()
  end
  return
 end
```

At the end of `_draw()`, add another branch for drawing the not started state:
```lua
if (p.gameover) then
  -- ...
 elseif (not_started) then
  camera()
  print("~Your title here~",8,60,10)
  print("press 🅾️ to start",28,68, 10)
 else
  -- draw player
  -- ...
 end
```

### 4. Increase the difficulty as the level increases
Include level in the calculations to determine the odds of lava or thorns. Be creative! Here's some ideas:

```lua
function gen_floor_height()
 -- negative = no floor
	local has_flr = rnd(8+level)-(level) > 0
	if (not has_flr) then
		return 0
	end
	return flr(rnd(3 + level * 0.34)) + 1
end

function has_thorns()
	return rnd(100)-(97-(level*2)) > 0
end
```

### Bonus:
- Add SFX for changing levels and/ or dying
