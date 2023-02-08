# Pygame image library
Adapted from https://www.pygame.org/docs/ref/image.html.

Note that these notes are focused on working with current pygame (as of time notes were taken) with the extended features available.

* `pygame.image.load_extended()` - loads an image source to generate a Surface; throws an exception if pygame extensions absent
    - `load_extended(filename) -> Surface`
    - `load_extended(fileobj, namehint="") -> Surface` load from a filelike object; namehint to provide filetype
* `pygame.image.save_extended()` - save a Surface to an image; throws an exception if pygame extensions absent
    - `save_extended(Surface, filename)`
    - `save_extended(Surface, fileobj, namehint="")`