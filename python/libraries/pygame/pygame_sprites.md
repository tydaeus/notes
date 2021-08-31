# Pygame Sprites
(notes adapted from https://www.pygame.org/docs/ref/sprite.html#pygame.sprite.Sprite and the pygame tutorials)

The Pygame `sprite` module provides the `Sprite` class and several `Group` classes.

A `Sprite` represents an in-game object, while `Group`s provide ways to organize these objects.

## `Sprite`
Custom in-game objects should subclass `Sprite`, and their constructors should invoke the default `Sprite` constructor.

`Sprite` built-in methods (call original version if overriden):
* `add` - add the sprite to `Group`s
* `remove` - remove the sprite from `Group`s
* `kill` - remove the Sprite from all `Group`s
* `alive` - does the sprite belong to any `Group`s
* `groups` - list of `Group`s that contain this Sprite

`Sprite` built-in properties (populate these):
* `rect` - a `Rect` representing the `Sprite`'s current position and dimensions
* `image` - current image representation of sprite

`Sprite` abstract method (override):
* `update` - updates the `Sprite`'s current state


``` Python
class MySprite(pygame.sprite.Sprite):

    def __init__(self):
        pygame.sprite.Sprite.__init__(self)  # call Sprite intializer
        # ensure rect and image are populated with initial values
    
    def update(self):
        # update current state, shifting self.rect and/or self.image as necessary
```



## `Group`
`Group`s allow for logically grouping `Sprite`s and performing operations involving multiple `Sprite`s.

