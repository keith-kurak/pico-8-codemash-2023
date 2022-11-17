# Generating a random map
## Goal
Generate a random map and keep the player moving throughout it indefinitely.
## Tasks
1. Generate random map for non-overlapping spaces
2. Covertly relocate player so it seems like they just keep going through additional map
3. Generate random map for the overlapping spaces to complete the effect
## Useful info
### New API's

## How to code it
We will put the map generation functions on code tab 2 to keep things somewhat tidy.
### 1. Add simple map generation function
Add this to code tab 2. `gen_floor_height()` is a bit of slightly-premature optimization that we'll use in a minute
```lua
function gen_map()
 -- set flat space at begin/ end
 for i=0,15 do
		set_map_x(1,i)
 	set_map_x(1,i+112)
 end
	-- set variable space
 for i=16,111 do
  local flr_hgt = gen_floor_height()
  set_map_x(flr_hgt,i)
 end
end

function gen_floor_height()
 -- negative = no floor
 return rnd(6)-2
end

-- set a single column on map
function set_map_x(flr_hgt,x)
	-- clear
 for j=0,15 do
  mset(x,j,3)
 end
 for j=0,flr_hgt do
  -- negative nums make no floors
	mset(x,15-j,1)
 end
end
```
Then add this to the end of your `_init()`:
```lua
 gen_map()
```
At this point, you should be able to run the program and run all the way to the end of the map.
### 2. Keep generating more map
At the end of `update60()` add this to generate more map and reset the player's position back to the beginning once they reach the point where the end would be visible by the camera:
```lua
if p.x >= (127-12)*8 then
 p.x = 24
 gen_map()
end
```
Now, you should be able to run the program and run to the end of the map _and_ keep running. But the seams will be obvious, because the same map is used for the beginning/ end of each loop.

### 3. Remove the seams
Add this to tab 2:
```lua
function gen_missing_map()
 for i=0,15 do
  local flr_hgt = gen_floor_height()
	set_map_x(flr_hgt,i)
	set_map_x(flr_hgt,i+112)
 end
end
```
Also update `gen_map()` as such:
```diff
--function gen_map()
++ function gen_map(init)
 -- set flat space at begin/ end
++ if (init) then
 for i=0,15 do
	set_map_x(1,i)
--set_map_x(1,i+112)
 end
++end
	-- set variable space
 for i=16,111 do
```

Update `_init()`:
```diff
--gen_map()
++gen_map(true)
++needs_missing_map = true
```

Update the map generation calls in `_update60()`:
```diff
if p.x >= (127-12)*8 then
 p.x = 24
++needs_missing_map = true
 gen_map()
end
++if needs_missing_map and p.x >= (36-12)*8 then
++ gen_missing_map()
++ needs_missing_map = false
++end
```