# README (toy programs)

This is a prototype project for exploring some toy program ideas. 

## Goals:

1. Toy programs exist to teach, inspiring learning, and entertain, in that order.
2. Toys should be fun.
3. Each toy is fully self contained.
4. Toys may communicate with other toy programs in this project, but each should stand on their own for features.
5. Toys have constraints that are obvious in their form and function.
6. Constraints help teach and inspire.
7. Toy programs should encourage exploration and play.
8. Toys should look like what they are.
9. Toys should share common experience elements (icons, placement of teaching tools, menus, interactions).
10. Toy programs can progress one to the next (e.g., CPU basics, serial protocols, storage, etc.)

## Key architectural considerations

1. The project should use an existing, cross-platform game platform (e.g., Godot, Pygame, Unity, etc.)
2. The underlying toy program concepts should be abstracted into shared code, to reuse between toys:
    1. the teaching portions (which should rely on markdown, common folder structure, etc., similar to Ren'Py but for teaching)
    2. the toy portions (e.g., common networking capabilities, common ux, common visualization stylings)

## Candidate toy projects

See `docs/IDEAS.md` for the current candidate toy projects.

## Unknowns, questions, etc.

1. This project needs a name, but for now "toy programs" captures the intent.