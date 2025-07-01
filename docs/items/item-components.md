---
title: Item Components
description: Item components are used to change how your item appears and functions in the world.
category: General
nav_order: 2
mentions:
    - SmokeyStack
    - QuazChick
---

:::tip FORMAT & MIN ENGINE VERSION `1.21.90`
Using the latest format version when creating custom items provides access to fresh features and improvements. The wiki aims to share up-to-date information about custom items, and currently targets format version `1.21.90`.
:::

## Applying Components

Item components are used to change how your item appears and functions in the world. They are applied in the `components` child of `minecraft:item`.

<CodeHeader>BP/items/custom_item.json</CodeHeader>

```json
{
    "format_version": "1.21.90",
    "minecraft:item": {
        "description": {
            "identifier": "wiki:custom_item",
            "menu_category": {
                "category": "items"
            }
        },
        "components": {
            "minecraft:icon": {
                "textures": {
                    "default": "wiki:custom_item"
                }
            }
        }
    }
}
```

## List of Components

### Allow Off Hand

Determine whether an item can be placed in the off-hand slot of the inventory.

Type: Boolean

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:allow_off_hand": true
```

### Block Placer

Block Placer item component. Items with this component will place a block when used. Released from experiment in format version 1.20.10.

When used in Survival Mode, the item will be consumed.

Type: Object

-   `block`: String/Object
    -   Defines the block that will be placed.
-   `replace_block_item`: Boolean
    -   Learn more about replacing block items [here](/blocks/blocks-as-items#replacing-block-items).
-   `use_on`: Array
    -   List of block descriptors that contain blocks that this item can be used on. If left empty, all blocks will be allowed. See Custom Item Use Priority for more information on use behavior.
    -   This applies to Creative Mode as well.

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:block_placer": {
    "block": "wiki:custom_block",
    "use_on": [
        "minecraft:dirt",
        "wiki:custom_dirt"
    ]
}
```

### Bundle Interaction

Enables the bundle interface and functionality on the item.
The item must have the `minecraft:storage_item` component for this component to function.

_Released from experiment `Bundles` for format versions 1.21.40 and higher._

Type: Object

-   `num_viewable_slots`: Integer (1-64)
    -   Defines the maximum number of item stacks accessible from the top of the bundle.
    -   Slots are accessed in rows filling from the bottom of the tooltip from right to left.

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:bundle_interaction": {
    "num_viewable_slots": 12
}
```

### Can Destroy in Creative

Determines if the item will break blocks in Creative Mode while swinging.

Type: Boolean

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:can_destroy_in_creative": true
```

### Compostable

Allows this item to be used in a composter.

Type: Object

-   `composting_chance`: Float (0-100)
    -   How likely the compost level is to increase as a percentage.

_Released from experiment `Upcoming Creator Features` for format versions 1.21.60 and higher._

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:compostable": {
    "composting_chance": 50 // 50% chance to increment the compost level
}
```

### Cooldown

Cool down time for a component. After use, all items in a specified 'cool down category' become unusable for a determined amount of time defined in the component. Released from experiment in format version 1.20.10.

Requires `minecraft:use_modifiers`.

Type: Object

-   `category`: String
    -   The type of cool down for this item.
-   `duration`: Float
    -   The duration of time (in seconds) items with a matching category will spend cooling down before becoming usable again.
    -   If this value is a negative number, it renders the item unusable.

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:cooldown": {
    "category": "attack",
    "duration": 0.2
}
```

### Damage

Determines how much extra damage the item does on attack. How much extra damage the item does on attack. Note that this must be a positive value.

The actual damage the entity will receive is `value + 1` per the docs "extra damage" due to the hand/item having a default value of 1 damage.
Damage value is `value % 256`.
Uses signed 16-bit integer. 2’s complements creates negative range.
`[32768-65536]` - gets treated as negative. The values given to the item will be `(-32768-0)`. So the negative ranges are `[256*(256x+128) - 256*(256(x+1)))`, where `x` is any arbitrary number.

https://bugs.mojang.com/browse/MCPE-180073

Type: Integer

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:damage": 10
```

### Damage Absorption

Causes the item to absorb damage that would otherwise be dealt to its wearer. For this to happen, the item needs to have the durability component and be equipped in an armor slot.

Type: Object

-   `absorbable_causes`: Array
    -   List of damage causes (such as `entity_attack` and `magma`) that can be absorbed by the item.

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:damage_absorption": {
    "absorbable_causes": ["all"]
}
```

### Digger

Determine how quickly an item can dig specific blocks.

Type: Object

-   `destroy_speeds`: Object - A list of blocks to dig, with correlating speeds of digging.
    -   `block`: String/Object - What block the item will destroy.
        -   `tags`: String
            -   Molang query
    -   `speed`: Integer
        -   How fast the block will be destroyed.
        -   Can be negative. If negative, the item will not be able to destroy the block.
-   `use_efficiency`: Boolean
    -   Determines whether the item should be impacted if the `efficiency` enchant is applied to it.
    -   Does not seem to work.

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:digger": {
    "use_efficiency": true,
    "destroy_speeds": [
        {
            "block": {
                "tags": "q.any_tag('stone', 'metal')" // Note that not all blocks have tags; listing many blocks may be necessary
            },
            "speed": 6
        }
    ]
}
```

### Display Name

Defines the text shown when an item name is shown, such as hover text. Released from experiment in format version 1.20.0.

Does not support newline escape character

Type: String

#### Example

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:display_name": {
    "value": "secret_weapon"
}
```

#### Example Using Localization Key

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:display_name": {
    "value": "item.snowball.name"
}
```

### Durability

Determines how much damage the item can take before breaking, and allows the item to be combined in crafting. Released from experiment in format version 1.20.0.

Durability does not implicitly damage itself when mining blocks. It must be handled via ScriptAPI. It does however implicitly damage itself when damaging mobs.
Each hit on a mob decreases durability by 2. This does not match vanilla property for weapons, but does match vanilla property for tools.

When used with `minercraft:wearable`, hitting a mob with the item does not decrease durability by 2. Instead, it implicitly decreases durability by 1 when equipped and hit by an entity. This matches vanilla property.

https://bugs.mojang.com/browse/MCPE-180112

Type: Object

-   `damage_chance`: Object - Damage chance is the percentage chance of this item losing durability. Default is set at 100. Defined as an int range with min and max value.
    -   `min`: Integer
        -   Minimum chance for durability to take damage. Range: [0, 100].
    -   `max`: Integer
        -   Maximum chance for durability to take damage. Range: [0, 100].
-   `max_durability`: Integer
    -   Max durability is the amount of damage that this item can take before breaking. This is a required parameter with a minimum value of 0.
    -   Uses signed 16-bit integer. 2’s complements creates negative range. `[32768-65536]` - gets treated as negative. The values given to the item will be `(-32768-0)`. So the negative ranges are `[256*(256x+128) - 256*(256(x+1)))` where x is an arbitrary number.
    -   https://bugs.mojang.com/browse/MCPE-180112

#### Damage Chance

Used to calculate unbreaking chance.

-   No Unbreaking - 100% of the time regardless of the range
-   Unbreaking I - 50% of the range
-   Unbreaking II - 33% of the range
-   Unbreaking III - 25% of the range

Max cannot be greater than min

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:durability": {
    "damage_chance": {
        "min": 0,
        "max": 100
    },
    "max_durability": 100
}
```

### Durability Sensor

Enables an item to emit effects when it receives damage.

Type: Object

-   `durability_thresholds`: Array
    -   Items define both the durability thresholds, and the effects emitted when each threshold is met.
    -   When multiple thresholds are met, only the threshold with the lowest durability after applying the damage is considered.

#### Durability Threshold

Type: Object

-   `durability`: Integer
    -   The effects are emitted when the item durability value is less than or equal to this value.
-   `particle_type`: String
    -   Particle effect to emit when the threshold is met.
-   `sound_event`: String
    -   Sound effect to emit when the threshold is met.

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:durability_sensor": {
    "durability_thresholds": [
        {
            "durability": 100,
            "particle_type": "minecraft:explosion_manual",
            "sound_event": "blast"
        },
        {
            "durability": 5,
            "sound_event": "raid.horn"
        }
    ]
}
```

### Dyeable

Allows the item to be dyed by cauldron water. Once dyed, the item will display the `dyed` texture defined in the `minecraft:icon` component rather than `default`.

Type: Object

-   `default_color`: String
    -   Optional color to use by default before the player has dyed the item.

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:dyeable": {
    "default_color": "#ffffff"
}
```

### Enchantable

Determines what enchantments can be applied to the item. Not all enchantments will have an effect on all item components.

https://bugs.mojang.com/browse/MCPE-180331

Type: Object

-   `slot`: String
    -   What enchantments can be applied (ex. Using `bow` would allow this item to be enchanted as if it were a bow).
    -   Required Field.
-   `value`: Integer
    -   The value of the enchantment (minimum of 0).
    -   Required Field
    -   Value is `value % 256`

#### Enchantable Slots

| Slot Name       |
| --------------- |
| `armor_feet`    |
| `armor_torso`   |
| `armor_head`    |
| `armor_legs`    |
| `axe`           |
| `bow`           |
| `cosmetic_head` |
| `crossbow`      |
| `elytra`        |
| `fishing_rod`   |
| `flintsteel`    |
| `hoe`           |
| `pickaxe`       |
| `shears`        |
| `shield`        |
| `shovel`        |
| `sword`         |
| `all`           |

#### Enchantability Value

Determines the item's enchantability, influencing the quality and quantity of potential enchantments. Higher values boost the chance of guaranteeing more powerful enchantments. The table below details enchantability scores across different materials, showcasing their ability to get enchantments.

| Material  | Armor Enchantability | Sword/Tool Enchantability |
| --------- | -------------------- | ------------------------- |
| Wood      | N/A                  | 15                        |
| Leather   | 15                   | N/A                       |
| Stone     | N/A                  | 5                         |
| Chain     | 12                   | N/A                       |
| Iron      | 9                    | 14                        |
| Gold      | 25                   | 22                        |
| Diamond   | 10                   | 10                        |
| Turtle    | 9                    | N/A                       |
| Netherite | 15                   | 15                        |
| Other     | 1                    | 1                         |

For an in-depth exploration of enchantability and its impact on the game, refer to the [Enchanting Mechanics on the Unofficial Minecraft Wiki](https://minecraft.wiki/w/Enchanting_mechanics#Enchantability).

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:enchantable": {
    "slot": "sword",
    "value": 10
}
```

### Entity Placer

Allows the item to place specified entities into the world. Released from experiment in format version 1.20.0.

Type: Object

-   `dispense_on`: Array
    -   List of block descriptors that contain blocks that this item can be dispensed on. If left empty, all blocks will be allowed.
    -   The mouth of the dispenser has to be pointing to either an air block or the block defined in this array. If it’s an air block, the game checks if the block below matches a block defined in this array
-   `entity`: String
    -   The entity to be placed in the world.
-   `use_on`: Array
    -   List of block descriptors that contain blocks that this item can be used on. If left empty, all blocks will be allowed. See Custom Item Use Priority for more information on use behavior.

<CodeHeader>minecraft:item > components</CodeHeader>

```json
    "minecraft:entity_placer": {
        "entity": "minecraft:spider",
        "dispense_on": [
            "minecraft:dirt"
        ],
        "use_on": [
            "minecraft:dirt"
        ]
    }
```

### Food

When an item has a food component, it becomes edible to the player. Must have the `minecraft:use_modifiers` component in order to function properly.

Will implicitly play the eating animation in third person. First person requires `minecraft:use_animation`.

Type: Object

-   `can_always_eat`: Boolean
    -   If `true` you can always eat this item (even when not hungry).
-   `nutrition`: Integer
    -   The value that is added to the actor's nutrition when the item is used.
    -   Can be negative.
    -   Max value is the 32-bit integer limit
-   `saturation_modifier`: Float
    -   Saturation Modifier is used in this formula: `(nutrition * saturation_modifier * 2)` when applying the saturation buff.
    -   Value must be greater than 0
-   `using_converts_to`: String
    -   When used, converts to the item specified by the string in this field.

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:food": {
    "can_always_eat": false,
    "nutrition": 3,
    "saturation_modifier": 0.6,
    "using_converts_to": "bowl"
}
```

### Fuel

Allows the item to be used as fuel in a furnace to 'cook' other items. Released from experiment in format version 1.20.0.

Max value is `107374180` inclusive. The reason for this number is because when translated to ticks, it hits the 32-bit integer limit

Type: Object

-   `duration`: Float
    -   How long in seconds will this fuel cook items for. Minimum value: 0.05.

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:fuel": {
    "duration": 3.0
}
```

### Glint

Determines whether the item has the enchanted glint render effect on it.

Type: Boolean

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:glint": false
```

### Hand Equipped

Determines if an item is rendered like a tool while in-hand.

Type: Boolean

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:hand_equipped": true
```

### Hover Text Color

Determines the color of the item name when hovering over it.

Valid colors can be found here: https://minecraft.wiki/w/Formatting_codes#Color_codes

Type: String

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:hover_text_color": "minecoin_gold"
```

### Icon

Determines the icon to represent the item in the UI and elsewhere. Released from experiment in format version 1.20.10.

Type: Object

-   `textures`: Object - This map contains the different textures that can be used for the item's icon. Armor trim textures and palettes can be specified here as well. The icon textures are the keys from the `resource_pack/textures/item_texture.json -> texture_data` object associated with the texture file.
    -   `default`
        -   The actual icon used for items
    -   `dyed`
        -   The icon displayed after a [dyeable](#dyeable) item is dyed in a cauldron.
    -   `icon_trim`
        -   The icon overlay for when your item has a trim on it.
        -   `icon_trim` implicitly falls back to the type of slot in the `minecraft:wearable` component. Currently, the icon will only overlay if the shortname matches the item’s identifier. Whether this is a bug or feature is unknown yet.
    -   `bundle_open_back`
        -   The texture displayed behind the preview of the item selected in the [bundle interaction](#bundle-interaction) tooltip.
    -   `bundle_open_front`
        -   The texture displayed in front of the preview of the item selected in the [bundle interaction](#bundle-interaction) tooltip.

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:icon": {
    "textures": {
        "default": "wiki:custom_item"
    }
}
```

### Interact Button

This component is a boolean or string that determines if the interact button is shown in touch controls and what text is displayed on the button. When set to `true`, default "Use Item" text will be used.

Type: Boolean/String

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:interact_button": "Use This Custom Item"
```

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:interact_button": true
```

### Liquid Clipped

Determines whether an item interacts with liquid blocks on use.

When interacted with liquid, the outline selection cannot be highlighting any blocks below the liquid. The interaction takes place inside the liquid block, not on its sides.

Type: Boolean

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:liquid_clipped": true
```

### Max Stack Size

Determines how many of an item can be stacked together.

Type: Integer

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:max_stack_size": 64
```

### Projectile

Projectile item component. Projectile items shoot out, like an arrow. Released from experiment in format version 1.20.10.

Type: Object

-   `minimum_critical_power`: Float
    -   Defines the time a projectile needs to charge in order to critically hit.
-   `projectile_entity`: String
    -   The entity to be fired as a projectile. If no namespace is specified, it is assumed to be `minecraft`.

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:projectile": {
    "minimum_critical_power": 1.25,
    "projectile_entity": "arrow"
}
```

### Rarity

Represents how difficult the item is to obtain by changing the color of its name text.
This component will be overridden and have no effect if the `minecraft:hover_text_color` is also applied.

An item's rarity value will be upgraded to `rare` if enchanted, or `epic` if its base rarity is already `rare`.
An `epic` rarity item will remain unchanged when enchanted.

Type: String

-   `common` results in a white name.
-   `uncommon` results in a yellow name.
-   `rare` results in an aqua name.
-   `epic` results in a light purple name.

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:rarity": "rare"
```

### Record

The record item component allows the item to play a sound when used in a jukebox.

Type: Object

-   `comparator_signal`: Tnt
    -   Signal strength for comparator blocks to use
    -   While this value can be any number(even negative!), the comparator signal still locks itself between [0, 15].
-   `duration`: Float
    -   Duration of sound event in seconds float value.
-   `sound_event`: String
    -   Sound event types
    -   If sound type is any of the vanilla music discs, the item will have the lore of the artist’s name. When played in a jukebox, an actionbar will appear on the player’s screen describing what record is being played.
    -   Regardless of what sound type used, the item’s text color will change to aqua just like vanilla music discs
    -   Only vanilla sound events are allowed

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:record": {
    "comparator_signal": 1,
    "duration": 5,
    "sound_event": "ambient.tame"
}
```

#### Sound Event

Listed [here](https://learn.microsoft.com/en-us/minecraft/creator/reference/content/itemreference/examples/itemcomponents/minecraft_record?view=minecraft-bedrock-stable) are the available sounds

### Repairable

Repairable item component: Determines which items can be used to repair a defined item, as well as the amount of durability specified items will repair. Released from experiment in format version 1.20.10.

By default, it can be repaired by itself. It will combine the two durabilities.

Type: Object

-   `repair_items`: Array - List of repair item entries.
    -   `repair_amount`: Int/String
        -   How much durability is repaired
        -   Molang can be used in the string type. `math.random` can be used. Use `context.other` to get the anvil’s second slot.
    -   `items`: Array
        -   The items used to repair the item
        -   Required Field

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:repairable":{
    "repair_items": [
        {
            "items":[
                "minecraft:diamond"
            ],
            "repair_amount": 10
        }
    ]
}
```

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:repairable":{
    "repair_items": [
        {
            "items":[
                "minecraft:diamond"
            ],
            "repair_amount": "math.random(1,10)"
        }
    ]
}
```

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:repairable":{
    "repair_items": [
        {
            "items":[
                "minecraft:diamond"
            ],
            "repair_amount": "math.min(q.remaining_durability + c.other->q.remaining_durability + math.floor(q.max_durability /20), c.other->q.max_durability)" // Vanilla formula
        }
    ]
}
```

### Shooter

Shooter Item Component. Must have the `minecraft:use_modifiers` component in order to function properly. Released from experiment in format version 1.20.10.

Type: Object

-   `ammunition`: Array
    -   `item`: String - Denotes the item description identifier. Item must have the `minecraft:projectile` component.
    -   `use_offhand`: Boolean - When set to `true`, ammunition can be used from the offhand.
    -   `search_inventory`: Boolean - Determines whether the inventory can be searched for ammunition to use. Required to be `true` if used in Survival. In Creative, required to be true if `use_in_creative` is `false`. Will not consume ammunition if in Creative.
    -   `use_in_creative`: Boolean - Determines whether the ammunition can be used in Creative mode.
-   `charge_on_draw`: Boolean
    -   Sets if the item is charged when drawn
    -   Item's `minecraft:use_modifiers` -> `use_duration` must be >= `max_draw_duration`.
-   `max_draw_duration`: Float
    -   Determines how long can the weapon can be drawn before releasing automatically
-   `scale_power_by_draw_duration`: Boolean
    -   When set to `true`, the longer the weapon is drawn, the more power it will have when released

#### Ammunition

Sets the entity that is used as ammunition. The priority of what item to use is as follows: First, it will check the off hand slot to see if it matches any of the ammunition. If matched, it will see if that object has `use_offhand` set to `true`. If no items match the offhand, it will go through the array in order, regardless of the order in the inventory.

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:shooter": {
    "ammunition": [
        {
            "item": "custom_projectile",
            "use_offhand": true,
            "search_inventory": true,
            "use_in_creative": true
        }
    ],
    "max_draw_duration": 1.0,
    "scale_power_by_draw_duration": true,
    "charge_on_draw": false
}
```

### Should Despawn

Determines whether an item should eventually despawn while floating in the world.

Type: Boolean

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:should_despawn": true
```

### Stacked by Data

Determines if the same item with different aux values can stack. Additionally, defines whether the item actors can merge while floating in the world.

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:stacked_by_data": true
```

### Storage Item

Allows the item to act as a container and store other items.
The item must have a max stack size of 1 for this component to function.

_Released from experiment `Bundles` for format versions 1.21.40 and higher._

Type: Object

-   `allow_nested_storage_items`: Boolean
    -   Determines whether other storage items can be placed into the container.
-   `allowed_items`: Array
    -   Defines the items that are exclusively allowed in the container.
    -   If empty all items are allowed in the container.
-   `banned_items`: Array
    -   Defines the items that are not allowed in the container.
-   `max_slots`: Integer (1-64)
    -   Defines the number of slots in the container.
-   `max_weight_limit`: Integer

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:storage_item": {
    "max_slots": 64,
    "allow_nested_storage_items": true,
    "banned_items": [
        "minecraft:shulker_box",
        "minecraft:undyed_shulker_box"
    ]
}
```

### Storage Weight Limit

Defines the maximum allowed total weight of all items in the storage item container.
The item must have the `minecraft:storage_item` component for this component to function.

-   To calculate the weight of an item, divide 64 by its max stack size.
-   Items that stack to 64 weigh 1 each, those that stack to 16 weigh 4 each and unstackable items weigh 64.

Type: Integer

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:storage_weight_limit": 64
```

### Storage Weight Modifier

Defines the additional weight the item adds when inside another storage item.

-   A value of 0 means that this item is not allowed inside another storage item.

Type: Integer (0-64)

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:storage_weight_modifier": 4
```

### Tags

The `tags` component determines which tags are attached to an item.

Type: Object

-   `tags`: Array
    -   List of tags attached to the item.

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:tags": {
    "tags": [
        "custom_tag"
    ]
}
```

### Throwable

Throwable item component. Throwable items, such as a snowball. Released from experiment in format version 1.20.10. Item must have the `minecraft:projectile` component.

Type: Object

-   `do_swing_animation`: Boolean
    -   Whether the item should use the swing animation when thrown.
-   `launch_power_scale`: Float
    -   The scale at which the power of the throw increases.
    -   Can be negative. Negative values will launch the projectile the opposite way.
-   `max_draw_duration`: Float
    -   The maximum duration to draw a throwable item.
    -   Can be negative. Will shoot instantly.
    -   No side effects if max is less than min from testing.
-   `max_launch_power`: Float
    -   The maximum power to launch the throwable item.
    -   Can be negative.
-   `min_draw_duration`: Float
    -   The minimum duration to draw a throwable item.
    -   Can be negative. Will shoot instantly.
-   `scale_power_by_draw_duration`: Boolean
    -   Whether or not the power of the throw increases with duration charged. When `true`, The longer you hold, the more power it will have when released.

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:throwable": {
    "do_swing_animation": false,
    "launch_power_scale": 1.0,
    "max_draw_duration": 0.0,
    "max_launch_power": 1.0,
    "min_draw_duration": 0.0,
    "scale_power_by_draw_duration": false
}
```

### Use Animation

Determines which animation plays when using an item.

Type: String

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:use_animation": "eat"
```

#### Use Animations

| Animation Name |
| -------------- |
| `eat`          |
| `drink`        |
| `bow`          |
| `block`        |
| `camera`       |
| `crossbow`     |
| `none`         |
| `brush`        |
| `spear`        |
| `spyglass`     |

### Use Modifiers

Modifies use effects, including how long an item takes to use and the player's speed when used in combination with components like Shooter, Throwable or Food.

Type: Object

-   `movement_modifier`: Float
    -   Modifier value to scale the players movement speed when item is in use.
    -   Range: [0, 1]
-   `use_duration`: Float
    -   How long the item takes to use in seconds.
    -   Required Field

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:use_modifiers": {
    "movement_modifier": 0.5,
    "use_duration": 1.0
}
```

### Wearable

Determines where the item can be worn. If any non-hand slot is chosen, the max stack size is set to 1.

Type: Object

-   `slot`: String
-   `protection`: Integer
-   `hides_player_location`: Boolean
    -   Determines whether a player wearing the item will be hidden from the Locator Bar and Locator Maps.

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:wearable": {
    "slot": "slot.armor.chest",
    "protection": 10
}
```

#### Wearable Slots

| Slot Name             |
| --------------------- |
| `slot.weapon.offhand` |
| `slot.armor.head`     |
| `slot.armor.chest`    |
| `slot.armor.legs`     |
| `slot.armor.feet`     |
