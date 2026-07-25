---
tags: 
- Python
- Projects
- Programming-Language
MOC: Personal
---
[[_0000 Home|Home]] | [[_0003 Personal MOC]] | [[PER Project ideas index|Back to index]]

# Current progrogress
- The map owns the rendering
- Snake owns the game logic
- Continue the separation between logic and rendering
# Buttons
- Play
- Pause
# Events
- Game starts 
	- Default snake spawns,
	- Apple spawns
- Apple eaten
	- Spawns a new one when eaten
	- Increases snake size by 1
- Wall hit 
	- Snake dies
- Tail hit 
	- Snake dies
	- Optional: Snake size decreases depending on where the collision happened
# Entities
- Snake 
	- 4 blinking braille dots
	- shape shifts when it moves
	- Can go up, down, left, right
	- Can't go up if moving down and vice versa
	- Can't go left if moving right and vice versa
- Apple
	- Single blinking braille dot
# Map
- Grid of varying sizes
	- Should be a grid of braille dots with size options
# Game logic
- Snake braille dots should be stored as coordinates on the grid in an array
- The array updates as the snake moves, each dot relative to the movement command
- The inputted movement should apply to each item in the array in turn (Snake is moving right and player inputs down at position 4x5? Each dot in the snake body must receive the command and pass by position 4x5 then move down)
- When the snake eats an apple, a new dot is added to the end of the list at the current coordinate of the last dot -1 in whichever movement direction the last dot currently has
- If the first dot in the list (snake head) gets a movement command to a coordinate outside the grid or reaches a coordinate point in which a part of the snake body exists, the game ends
# Game logic update
- I didn't use the grid idea at all
- Instead, I used the `offset` CSS style from textual, by supplying it with X and Y values
- It's basically the same thing as the grid but using the entire screen as my map
- Due to this I still need to add a border as the game space and define the border's and X or Y value past the border perimeter as a wall

# Achievements/Milestones
- [x] Movement
- [x] Controlled movement
- [x] Movement in all directions
- [ ] Snake body shape changes depending on movement
- [ ] Add apple
- [ ] Snake can eat apple
- [ ] Snake grows after eating apple
- [ ] Add border
- [ ] Snake dies when it hits the border
- [ ] Snake dies when it eats itself

# Refer to
- [[PG Python index]]