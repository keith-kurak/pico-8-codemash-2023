# Damage/ game over
## Goal
Create a failure state by creating tiles that cause damage
## Tasks
1. Replace empty spaces with lava
2. Generate random thorns/ spikes on land tiles for extra challenge
3. Detect collision with damage tiles and put player into "game over" state
## Useful info
### New API's
[PRINT](https://www.lexaloffle.com/dl/docs/pico-8_manual.html#PRINT)
```
PRINT(STR, X, Y, [COL])
PRINT(STR, [COL])
Print a string STR and optionally set the draw colour to COL.
```

## How to code it
### 1. Make a lava sprite

Give it a flag of 2. This will indicate something causes damage.

### 2. Fill in lava where there is no floors

Add this to the very end `set_map_x()`
```lua
-- make lava
	if flr_hgt < 1 then
	 mset(x,15,[LAVA SPRITE #])
	end

```

### 3. Create game over screen

In the player definition in `_init()`, add a property to track if the player is in a game over state:

```diff
player = {
 x = 64,
 y = 64,
 dx = 0,
 dy = 0,
 spd = 1,
 jump = 1,
 on_ground = false,
++gameover = false
}
```

In the `_draw()` function, add the game over screen
```lua
if (player.gameover) then
 camera()
 print("game over!",40,60)
 print("press 🅾️ to try again",20,68)
 return
end
```

If you add this after the map is drawn, but before the player is drawn, IMO it looks nice.

### 4. Check collision with "damage" object to set game over state
At the top of `_update60()`, apply gameover status based on hitting the lava/ allow player to restart the game:

```lua
-- already game over
 if p.gameover then
  if (btnp(🅾️)) then
   _init()
  end
  return
 end
 -- apply game over
 if hit(p.x,p.y,7,7,2) then
  p.gameover=true
  return
 end

```

### 5. Add thorns/ spikes

Create a new sprite to represent a damage object that will be on top of land (use the same flag).

Add a `has_thorns()` function on tab 2 to calculate the odds thorns will appear on land:
```lua
function has_thorns()
 return rnd(100)-90 > 0
end
```

In `gen_map()`, add this code to occasionally randomly put thorns on land:
```diff
for i=16,111 do
 local flr_hgt = gen_floor_height()
 set_map_x(flr_hgt,i)
++if (flr_hgt > 1 and has_thorns()) then
++mset(i,15-flr_hgt,3)
++end
end
```