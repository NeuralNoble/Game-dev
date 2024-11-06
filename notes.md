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

# Sprites

Using sprites in Pygame can greatly simplify the creation of games by organizing graphical elements and game logic. 
### What is a Sprite in Pygame?

In game development, a **sprite** is a 2D image or animation integrated into a larger scene. In Pygame, sprites are used to represent objects in your game, like characters, enemies, items, and obstacles. Pygame provides a `Sprite` class that helps manage these objects.

#### Why Use Sprites?
Sprites make it easier to:
- Group multiple game objects.
- Control and update multiple objects.
- Manage collisions.
- Handle rendering efficiently.

### Basic Components of a Sprite in Pygame

1. **Sprite Class**: You create a new class that inherits from `pygame.sprite.Sprite`. This lets you define properties and behaviors for your object.
2. **Image**: Every sprite needs an image to represent it visually.
3. **Rect**: The position and area of the sprite on the screen. Pygame’s `Rect` class makes it easy to control position, dimensions, and collisions.

### Setting Up Sprites in Pygame

Here’s a simple breakdown of how to set up a sprite.

1. **Initialize Pygame**: Import and initialize Pygame.
2. **Define the Sprite Class**: Create a custom class inheriting from `pygame.sprite.Sprite`.
3. **Add Image and Rect Attributes**: Load an image for the sprite and set its position using a `Rect`.
4. **Add an Update Method**: Define an update method for the sprite's behavior.
5. **Create a Group**: Use `pygame.sprite.Group()` to manage multiple sprites.

### Example Code: A Simple Sprite

```python
import pygame
import sys

# Initialize Pygame
pygame.init()

# Set up display
screen = pygame.display.set_mode((800, 600))
pygame.display.set_caption("Sprite Example")

# Colors
WHITE = (255, 255, 255)

# Define the Player Sprite
class Player(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        # Load an image for the sprite
        self.image = pygame.Surface((50, 50))
        self.image.fill((0, 128, 255))  # Fill it with a color (blue)
        self.rect = self.image.get_rect()
        self.rect.center = (400, 300)  # Start position

    def update(self):
        # Move sprite with arrow keys
        keys = pygame.key.get_pressed()
        if keys[pygame.K_LEFT]:
            self.rect.x -= 5
        if keys[pygame.K_RIGHT]:
            self.rect.x += 5
        if keys[pygame.K_UP]:
            self.rect.y -= 5
        if keys[pygame.K_DOWN]:
            self.rect.y += 5

# Create the sprite and add it to a group
player = Player()
all_sprites = pygame.sprite.Group()
all_sprites.add(player)

# Game loop
clock = pygame.time.Clock()
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    # Update all sprites
    all_sprites.update()

    # Clear the screen
    screen.fill(WHITE)
    
    # Draw all sprites
    all_sprites.draw(screen)
    
    # Flip the display
    pygame.display.flip()
    
    # Cap the frame rate
    clock.tick(60)
```

### Explanation of the Code

1. **Player Class**: The `Player` class inherits from `pygame.sprite.Sprite` and represents a blue square. The `__init__` method loads the image, sets up the rectangle (for position), and initializes the starting position.
2. **Update Method**: This method allows the player to move with arrow keys by adjusting the `rect` coordinates.
3. **Sprite Group**: `all_sprites` is a `Group` that manages all sprites in the game. By calling `all_sprites.update()` and `all_sprites.draw(screen)`, we update and draw all sprites in one line each.
4. **Game Loop**: In each frame, we handle events, update sprites, fill the screen, draw sprites, and update the display.

### Key Concepts in Pygame Sprites

#### 1. `pygame.sprite.Group()`
   - A `Group` is a container for managing multiple sprites. You can add, update, and draw sprites collectively. The main benefits of `Group` are efficiency and simplicity.

#### 2. Collision Detection
   - Pygame’s `spritecollide()` and `collide_rect()` make it easy to detect collisions between sprites in a `Group`.
   - Example:

     ```python
     if pygame.sprite.spritecollide(player, enemies_group, False):
         print("Collision Detected!")
     ```

#### 3. Animated Sprites
   - For animated sprites, load multiple frames and switch between them over time.

     ```python
     class AnimatedSprite(pygame.sprite.Sprite):
         def __init__(self, images):
             super().__init__()
             self.images = images  # List of images
             self.index = 0
             self.image = self.images[self.index]
             self.rect = self.image.get_rect()
             self.rect.center = (400, 300)

         def update(self):
             # Change image every few frames
             self.index += 1
             if self.index >= len(self.images):
                 self.index = 0
             self.image = self.images[self.index]
     ```

### Advanced Topics in Sprites

#### Using Multiple Groups
   - Sprites can belong to multiple groups, allowing you to manage them more flexibly. For example, you could have a group for all players, a group for enemies, and a master group containing all sprites.

     ```python
     player = Player()
     all_sprites = pygame.sprite.Group(player)
     enemies = pygame.sprite.Group()
     ```

#### Layered Updates with `LayeredUpdates`
   - Pygame has a `LayeredUpdates` group, which allows for managing sprites with different layers (useful for z-ordering).
   - Example:

     ```python
     group = pygame.sprite.LayeredUpdates()
     group.add(player, layer=1)
     ```

#### Scaling and Transforming Sprites
   - Use `pygame.transform.scale()` to resize sprites or `pygame.transform.rotate()` to rotate them.

     ```python
     self.image = pygame.transform.scale(self.image, (100, 100))
     ```

### Example: Adding Enemies

Let’s expand the example by adding enemy sprites and detecting collisions.

```python
import pygame
import sys
import random

pygame.init()
screen = pygame.display.set_mode((800, 600))
WHITE = (255, 255, 255)

# Player Sprite
class Player(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface((50, 50))
        self.image.fill((0, 128, 255))
        self.rect = self.image.get_rect()
        self.rect.center = (400, 300)

    def update(self):
        keys = pygame.key.get_pressed()
        if keys[pygame.K_LEFT]: self.rect.x -= 5
        if keys[pygame.K_RIGHT]: self.rect.x += 5
        if keys[pygame.K_UP]: self.rect.y -= 5
        if keys[pygame.K_DOWN]: self.rect.y += 5

# Enemy Sprite
class Enemy(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface((30, 30))
        self.image.fill((255, 0, 0))
        self.rect = self.image.get_rect()
        self.rect.x = random.randint(0, 770)
        self.rect.y = random.randint(0, 570)

# Create groups and add sprites
player = Player()
all_sprites = pygame.sprite.Group()
enemies = pygame.sprite.Group()

for _ in range(5):  # Add 5 enemies
    enemy = Enemy()
    all_sprites.add(enemy)
    enemies.add(enemy)

all_sprites.add(player)

# Game loop
clock = pygame.time.Clock()
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    # Update
    all_sprites.update()

    # Check collisions
    if pygame.sprite.spritecollide(player, enemies, False):
        print("Collision detected!")

    # Draw
    screen.fill(WHITE)
    all_sprites.draw(screen)
    pygame.display.flip()
    clock.tick(60)
```

In this code:
1. We added an `Enemy` class.
2. We created a group of enemies and added them to `all_sprites`.
3. `spritecollide()` checks if the player collides with any enemy, printing a message if a collision is detected.

### Summary

Pygame sprites are powerful for managing game objects efficiently. By organizing code into sprite classes and groups, you can create dynamic, interactive games that handle movement, collisions, and animations effectively.
