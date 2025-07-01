---
title: Block Traits
description: Block traits can be used to apply vanilla block states (such as direction) to your custom blocks easily, without the need for events and triggers.
category: General
nav_order: 5
license: true
mentions:
    - QuazChick
    - SmokeyStack
---

:::tip FORMAT & MIN ENGINE VERSION `1.21.90`
Before you learn about block traits, you should be confident with [block states](/blocks/block-states).

When working with block states, ensure that the `min_engine_version` in your pack manifest is `1.20.20` or higher.
:::

## Applying Traits

Block traits can be used to apply vanilla block states (such as direction) to your custom blocks easily, without the need for events and triggers.

<CodeHeader>BP/blocks/custom_slab.json</CodeHeader>

```json
{
    "format_version": "1.21.90",
    "minecraft:block": {
        "description": {
            "identifier": "wiki:custom_slab",
            "menu_category": {
                "category": "construction",
                "group": "minecraft:itemGroup.name.slab"
            },
            "traits": {
                "minecraft:placement_position": {
                    "enabled_states": ["minecraft:vertical_half"]
                }
            }
        },
        "components": { ... },
        "permutations": [ ... ]
    }
}
```

_This example will set the `minecraft:vertical_half` block state when placed to either `'top'` or `'bottom'` - depending on where the player is looking._

**Permutations are still required for this state to make a functional difference, with conditions querying**

```molang
q.block_state('minecraft:vertical_half')
```

## List of Traits

### Placement Direction

Contains information about the player's rotation when the block was placed.

_Requires format version [1.20.20](/blocks/block-format-history#_1-20-20) or later._

#### Provided States

| State                          | Values                                                                           | Description                                      |
| ------------------------------ | -------------------------------------------------------------------------------- | ------------------------------------------------ |
| `minecraft:cardinal_direction` | `"south"` _(default)_<br>`"north"`<br>`"west"`<br>`"east"`                       | Cardinal facing direction of player when placed. |
| `minecraft:facing_direction`   | `"down"` _(default)_<br>`"up"`<br>`"south"`<br>`"north"`<br>`"west"`<br>`"east"` | Overall direction of player when placed.         |

#### Additional Parameters

-   `y_rotation_offset` - This rotation offset only applies to the horizontal state values (north, south, east, west) . Only axis-aligned angles may be specified (e.g. 90, 180, -90).

<CodeHeader>minecraft:block > description > traits</CodeHeader>

```json
"minecraft:placement_direction": {
    "enabled_states": ["minecraft:cardinal_direction"],
    "y_rotation_offset": 180
}
```

### Placement Position

Contains information about where the player placed the block.

_Requires format version [1.20.20](/blocks/block-format-history#_1-20-20) or later._

#### Provided States

| State                     | Values                                                                           | Description                                   |
| ------------------------- | -------------------------------------------------------------------------------- | --------------------------------------------- |
| `minecraft:block_face`    | `"down"` _(default)_<br>`"up"`<br>`"south"`<br>`"north"`<br>`"west"`<br>`"east"` | Face on which the block was placed.           |
| `minecraft:vertical_half` | `"top"`<br>`"bottom"` _(default)_                                                | The vertical half where the block was placed. |

<CodeHeader>minecraft:block > description > traits</CodeHeader>

```json
"minecraft:placement_position": {
    "enabled_states": [
        "minecraft:block_face",
        "minecraft:vertical_half"
    ]
}
```
