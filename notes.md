# 1. Create Window 

## Display Surface 
The canvas that everything will be drawn on,you can only have one at a time 

## Event Loop
checks events(keyboard,mouse & controller input,timers).
This also includes pressing x to close the game 


## Displaying Graphics

- pygame can display graphics in 2 ways:
   * show an image or text via a surface
   * draw pixels

### Surface
- A surface in pygame is usually an image (png.jpg)
- a plain area or rendered text

### How to Create?
- **plain surface:** ```pyagame.Surface((width,height))```
- **imported surface:** ```pygame.image.load(path)```
- **Text surface:** ```font.render(text,AntiAlias,color)```

### Display Surface vs surface
The display surface is the main surface that we draw on and there can only be one and it is always visible.

A Regular surface is an image of some kind , you can have any number but they are only visible when attached to the display surface

## Rects 

- place surfaces more elegantly
- detect collisions 
- can be drawn
- rects are jus rectangles with a size and position 
- they also have lots of points Tuples with an x and y position
    * topleft
    * midtop
    * topright
    * midleft
    * center
    * midright
    * bottomleft
    * midbottom
    * bottomright


## Movement & Delta time

In the most basic sense , moving stuff is easy : you simply blit a surface in a different position on every frame 

### Refinement 
- vectors are an excellent way to store a direction
- Including delta time make the movement framerate independent 

### Framerate Independence

- depending on the computer your game could run at very different framerates
- since we update the position on every new frame we get faster movement on faster computers 

### Delta time

- the time it took your computer to render the current frame or one frame
- 