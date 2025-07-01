---
title: Block Texture Animation
description: Learn how to create flipbook (animated) textures for blocks.
category: Visuals
tags:
    - intermediate
mentions:
    - MedicalJewel105
    - SquisSloim
    - SmokeyStack
    - QuazChick
---

From this page you will learn:

-   How to apply flipbook textures to a block.
-   Which values you can apply in `RP/textures/flipbook_textures.json` and what they do.

## Applying Flipbook Textures

Flipbook textures are animated textures. Blocks like fire, water, lava and magma use them. You can use animated texture for your blocks too!
For the first time let's use magma's animated texture.
You can simply apply animated magma's texture to your block by changing `texture` value to one, defined in `Vanilla RP/textures/terrain_texture.json`:

```json
"magma": {
    "textures": "textures/blocks/magma"
}
```

<CodeHeader>BP/blocks/flipbook_block.json</CodeHeader>

```json
{
    "format_version": "1.21.90",
    "minecraft:block": {
        "description": {
            "identifier": "wiki:flipbook_block",
            "menu_category": {
                "category": "construction"
            }
        },
        "components": {
            "minecraft:geometry": "minecraft:geometry.full_block",
            "minecraft:material_instances": {
                "*": {
                    "texture": "magma" // Add it here.
                }
            }
        }
    }
}
```

![](/assets/images/blocks/flipbook-textures/animated_texture_1.gif)

Now it has animated texture!

## Defining Flipbook Textures

After making block have animated texture, it is time to figure out how it all works.

1. Minecraft takes name and path to texture defined in `terrain_texture.json`

<CodeHeader>RP/textures/terrain_texture.json</CodeHeader>

```json
{
    "resource_pack_name": "wiki",
    "texture_name": "atlas.terrain",
    "texture_data": {
        "magma": {
            "textures": "textures/blocks/magma"
        }
    }
}
```

2. Minecraft searches looks into `flipbook_textures.json` aiming to find animation parameters for this name (`magma`)

<CodeHeader>RP/textures/flipbook_textures.json</CodeHeader>

```json
[
    {
        "atlas_tile": "magma",
        "flipbook_texture": "textures/blocks/magma",
        "ticks_per_frame": 10
    }
]
```

`"atlas_tile"` here adds animation parameters to `magma` name, defined in terrain_texture file.

3. Minecraft uses this animated texture for blocks who have `magma` as texture.

## Flipbook Texture Parameters

While looking up for something in vanilla flipbook texture file, you may notice some additional parameters:

| Component          | Type             | Description                                                                                                 |
| ------------------ | ---------------- | ----------------------------------------------------------------------------------------------------------- |
| flipbook_texture   | string           | Path to texture.                                                                                            |
| atlas_tile         | string           | The shortname defined in the `terrain_texture.json` file.                                                   |
| atlas_index        | integer          | The index of the texture array inside the definition of that shortname.                                     |
| atlas_tile_variant | integer          | The variant of the block's texture array inside the shortname's block variation.                            |
| ticks_per_frame    | integer          | How fast frames should be changed. 20 ticks = 1 second.                                                     |
| frames             | array or integer | List with frame index to use on each frame, or the total number of frames to be repeated one after another. |
| replicate          | integer          | Sets the size of pixels. Default: 1.                                                                        |
| blend_frames       | boolean          | Defines should frames transition be smooth or not. Default: true.                                           |

### `atlas_index`

A component where you'll define the block texture index to animate.

<CodeHeader>RP/textures/terrain_texture.json#texture_data</CodeHeader>

```json
"dirt": {
    "textures": [
        "textures/blocks/dirt",
        "textures/blocks/coarse_dirt" // Imagine that this is the path you want to animate
    ]
}
```

Since path 2 has an animated texture, therefore you'll put `"atlas_index": 1` on the Dirt block's flipbook texture.

### `atlas_tile_variant`

A component where you'll define the block variant (which is registered to the `variations` array) to animate.

<CodeHeader>RP/textures/terrain_texture.json#texture_data</CodeHeader>

```json
"dirt": {
    "textures": [
        {
            "variations": [
                { "path": "textures/blocks/dirt_va" }, // Imagine that this is the block variation you want to animate
                { "path": "textures/blocks/dirt0" },
                { "path": "textures/blocks/dirt1" }
            ]
        }
    ]
}
```

Now let's say we wanted path 1 to be animated, now what you'll do here is to put `"atlas_tile_variant": 1` on the Dirt block's flipbook texture.

### `replicate`

Changes size of the peace of used texture. Can only take values that are multiples of two. If frame has smaller amount of pixels, extends them.

| `replicate` value | what it does                              |
| ----------------- | ----------------------------------------- |
| < 0               | Breaks animation                          |
| 0                 | Breaks animation & texture                |
| 2                 | Renders 1 / 4 pixels of frame             |
| x                 | Renders 1 / x<sup>2</sup> pixels of frame |

## Result

![](/assets/images/blocks/flipbook-textures/animated_texture_2.gif)

Now you can modify vanilla flipbook textures or create your own ones!
