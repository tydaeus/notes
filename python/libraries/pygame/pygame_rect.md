# pygame Rect
Pygame uses the `Rect` object type to represent rectangular areas in a wide variety of contexts.

Construction:
``` python
Rect(left, top, width, height) -> Rect
Rect((left, top), (width, height)) -> Rect
Rect(object) -> Rect
```

Pygame functions that accept a `Rect` will also accept the constructor parameters to create one.

The following attributes can be used to observe or modify a rect; each is an int or pair of ints:
* x, y, w, h
* top, left, bottom, right
* topleft, bottomleft, topright, bottomright
* midtop, midleft, midbottom, midright
* center, centerx, centery
* size, width, height
