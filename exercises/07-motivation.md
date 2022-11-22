# Player motivation!
## Goal
Keep the player from going backwards and motivate them to go forwards
## Tasks
1. Add the "evil wall" to keep the player from going backwards and falling off the map
2. Keep score
3. Save a high score
## Useful info
### New API's

## How to code it
### 1. Add sprite-to-sprite collision
In tab 1, add this function to test if two sprites hit each other:
```lua
function hit_spr(x1,y1,x2,y2)
 -- if left edge of A is to the right of the right edge of B, OR
 -- if left edge of B is to the right of the right edge of A
 if (x1 >= x2+8 or x2 >= x1+8) return false;

 -- if top of A is lower than bottom of B, OR
 -- if top of B is lower than bottom of A
	if (y1 >= y2+8 or y2 >= y1+8) return false;

	return true;
end
```
### 2. Draw the sprite for your evil wall
This will repeat all along the Y axis, so think spikes or fire or whatever

### 3. Add wall table and draw the wall
Add the wall in `_init()`:
```lua
wall = {
 x = 0,
 dx = 0.5
}
```

Then draw the wall:

```lua
for i=0,15 do
 spr(7,wall.x,i*8)
end
```

### 4. Move the wall towards the player

```lua
-- move it
wall.x = wall.x + wall.dx
-- let it catch up to player so they can't fly off the back of the map
if (p.x - wall.x > 64 or wall.x > p.x) then
 wall.x = p.x - 64
end
```

### 5. Set up high score storage
Add this at the top of your code to setup data storage:
```lua
cartdata("some_unique_str")
```

Load/ init the high score in `_init()`
```lua
hi_score = dget(0)
if (hi_score == nil) then
 hi_score = 0
end
```

### 6. Print/ update the high score during the game
High score will just be forward distance, so we'll store distance and massage the value for display.

Add `dist = 0` to the `player` table initialized in `_init()`.

Update `dist` at the end of `_update60()`:

```lua
p.dist = p.dist + p.dx
```

Calculate the score and print it at the end of `_draw()`:

```lua
camera()
local score = max(flr(player.dist/8),0)
if (score > hi_score) then
 hi_score = score
end
print("score",0,0,10)
print(score,26,0,7)
print("hi",50,0,10)
print(hi_score,66,0,7)
```

Notice that we update the high score when drawing. This is probably wrong! But it's easy. Feel free to fix it.

### 7. Save the high score on game over
Update the game over logic in `_update60()`:
```diff
if hit(p.x,p.y,7,7,2) or
 hit_spr(p.x,p.y,wall.x,p.y) then
 p.gameover=true
++dset(0,hi_score)
 return
end
```
