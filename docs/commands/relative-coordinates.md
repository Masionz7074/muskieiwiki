---
title: Coordinate System
category: General
mentions:
    - MedicalJewel105
    - Sprunkles137
    - 7dev7urandom
    - Hatchibombotar
    - TheItsNameless
    - QuazChick
description: Understanding relative coordinates.
---

## The Coordinate System

Minecraft stores the locations of blocks and entities in the world using a system of three-dimensional coordinates, each representing a value in a one-dimensional axis. They are stored in the format of X, then Y, and lastly Z. Whether you are placing structures and blocks, or teleporting and summoning entities, you can, and are sometimes required to, put in coordinates. They don't need to always be real values however; you can substitute world coordinates for relative values, either based in world space or local space.

![](/assets/images/commands/relative-coordinates/coordinates.png)

_You may already be familiar with coordinates if you've enabled the Show Coordinates world option!_

## Relative Coordinates (~)

Relative coordinates are represented using tildes in place of real coordinates, and represent a position that is relative to the world coordinates its located at. You may insert numbers after a tilde to add an offset to the current position. These can be mixed with world coordinates, but cannot be mixed with local coordinates.

Examples:

-   `~ ~ ~`: Current position with no changes.
-   `~5 ~-2 ~`: Current position with a 5-block X offset and a negative 2-block Y offset.

### Rotations

Relative coordinates can also be used in the context of rotations, where they represent a rotation that is relative to the current rotation it inherits from. These may also accept numbers after the tilde to add an offset to the current rotation.

Example: `~90 ~` will add 90° to the current yaw (y-rotation) value.

## Local Coordinates (^)

Local coordinates are similar to relative coordinates, but represent a position in local space, where the axes are based off of rotation. They take the form `^left ^up ^forward`; you can think of this as `~x ~y ~z` if both your yaw and pitch rotations are 0 (facing straight ahead, due south).

Like relative coordinates, you can insert numbers to produce an offset of the current position, in local space. If there is no entity to copy rotation from, the x- and y-rotations are assumed to be 0.

Examples:

-   `^10 ^ ^`: Current position with a 10-block offset to the left.
-   `^ ^1.5 ^1`: Current position with a 1.5-block offset upward and a 1-block offset forward.

## Additional Notes

-   The player's eye level is 1.62 blocks above their feet. (~ ~1.62 ~)
