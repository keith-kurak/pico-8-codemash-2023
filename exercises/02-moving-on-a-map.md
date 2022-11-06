# Moving about a map
## Goal
Move a sprite around with the D-pad
## Tasks
1. Create a "block" sprite
2. Draw a floor on the map
3. Draw the map on the screen
4. Move the camera with the player sprite
## Useful Info
- Map is 128x128 sprites (but shares bottom 64 rows with sprite sheet)
- On map editor, use the hand tool to pan, two finger scroll to zoom in/ out
### New API's
[MAP](https://www.lexaloffle.com/dl/docs/pico-8_manual.html#MAP)
```
MAP(CELL_X, CELL_Y, [SX, SY], [CELL_W, CELL_H], [LAYERS])
Draw section of map (starting from CELL_X, CELL_Y) at screen position SX, SY (pixels)
```
[CAMERA](https://www.lexaloffle.com/dl/docs/pico-8_manual.html#CAMERA)
```
Set a screen offset of -x, -y for all drawing operations

CAMERA() to reset
```
## How to code it
### 1. Make a map block sprite
Just like your first sprite, but make it some nondescript piece of land or rock or whatever.

### 2. Draw a map with your land sprite
Draw some lightly rolling hills within the first 16 vertical blocks. Go at least 32-64 horizontal blocks, so you can see what happens when you scroll.

### 3. Apply the map and camera to your draw function
This will move the camera as your sprite moves, and draw the entire map, so you can move freely about it.
```diff
  function _draw()
    cls()
+   camera(x-24,0)
+   map()
    spr(0,x,y)
  end
```
