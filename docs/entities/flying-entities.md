---
title: Flying Entities
category: Tutorials
tags:
    - intermediate
mentions:
    - SirLich
    - Joelant05
    - Dreamedc2015
    - MedicalJewel105
    - aexer0e
    - imsolucid
    - nebulacrab
    - Lufurrius
    - TheItsNameless
    - Halo333X
description: Learn how to make a flying behavior for your entity.
---

Whether making a plane or a dragon, adding controllability to flying entities will probably challenge most devs who haven't dabbled around this concept. Since there is no "right" way of adding a piloting mechanic to flying entities, I'll showcase 3 main workaround ways you can use to achieve this.

## Great Jump, Slow Fall

While not exactly "flying", setting the entity's jumping power high and giving it slow falling & speed effects as it falls is probably the most straightforward method.

To achieve this, we will need to add the `"minecraft:horse.jump_strength"` component to our entity. Adding this will allow you to control its jumping power and disable dismounting when the player presses the jump button.

<CodeHeader></CodeHeader>

```json
"minecraft:horse.jump_strength": {
    "value": 7
}
```

We can also use `"value"` as an object to utilize the **range bar** players will see when holding down the jump button.

<CodeHeader></CodeHeader>

```json
"minecraft:horse.jump_strength": {
    "value": { "range_min": 0.6, "range_max": 1.2 }
}
```

Now we will give it slow falling and speed as it's falling so that it doesn't instantly fall. To do this, we will make an animation controller and give it those effects when it's not on the ground as so:

(You can read a tutorial on how to use animation controllers to execute commands [here](/animation-controllers/entity-commands).)

<CodeHeader></CodeHeader>

```json
"controller.animation.dragon.flying":{
    "states":{
        "default":{
            "transitions":[
                {
                    "jumping":"!q.is_on_ground"
                }
            ]
        },
        "jumping":{
            "transitions":[
                {
                    "default":"q.is_on_ground"
                }
            ],
            "on_entry":[
                "/effect @s slow_falling 20000 0 true",
                "/effect @s speed 20000 10 true"
            ],
            "on_exit":[
                "/effect @s clear"
            ]
        }
    }
}
```

We'll also need to hook it up to our entity as so:

<CodeHeader></CodeHeader>

```json
"description":{
    "identifier":"wiki:dragon",
    "is_spawnable":true,
    "is_summonable":true,
    "is_experimental":false,
    "scripts":{
        "animate":[
            "flying"
        ]
    },
    "animations":{
        "flying":"controller.animation.dragon.flying"
    }
}
```

Now, we should have a mechanic at least resemblant of flying. You can change the values like jump_strength and speed, but the entity will always fall using this method.

## Controlling Through Looking

This is probably the most popular method of piloting flying entities, and unlike the first method, this one gives players control over the vertical movement of the entity so that you don't always have to fall every time you jump, with the downside being you can't look around freely without changing the entity's vertical trajectory.

This method detects the riding player's vertical rotation and applies levitation/slow_falling effects to the entity accordingly.

There are multiple ways of achieving that, but in this tutorial, we'll be using the target selectors `rym` (minimum y-rotation) and `ry` (maximum y-rotation) in a chain of repeating command-blocks to detect the player's pitch, and depending on the range, giving our entity levitation or slowly falling.

<CodeHeader></CodeHeader>

```
execute as @a[rxm=-90,rx=-25] run effect @e[type=wiki:dragon,r=1] levitation 1 6 true
execute as @a[rxm=-25,rx=-15] run effect @e[type=wiki:dragon,r=1] levitation 1 3 true
execute as @a[rxm=-15,rx=-5] run effect @e[type=wiki:dragon,r=1] levitation 1 2 true
execute as @a[rxm=-5,rx=20] run effect @e[type=wiki:dragon,r=1] levitation 1 1 true
execute as @a[rxm=20,rx=35] run effect @e[type=wiki:dragon,r=1] slow_falling 1 1 true
execute as @a[rxm=35,rx=90] run effect @e[type=wiki:dragon,r=1] clear
```

**Depending on how big your entity is and how far away the player's seat is from its pivot, you might need to change the radius `r` to a more significant value.**

After you run those commands in a repeating command block, you should control its vertical movement by looking up and down.
or you may use a simple animation controller and link it to the entity, so it always plays the function.

It's recommended that you link this animation controller to the player.

<CodeHeader></CodeHeader>

```json
{
    "format_version": "1.10.0",
    "animation_controllers": {
        "controller.animation.base": {
            "initial_state": "default",
            "states": {
                "default": {
                    "transitions": [
                        {
                            "base": "(1.0)"
                        }
                    ],
                    "on_entry": ["/function dragon_control"]
                },
                "base": {
                    "transitions": [
                        {
                            "default": "(1.0)"
                        }
                    ],
                    "on_entry": ["/function dragon_control"]
                }
            }
        }
    }
}
```

The entity will probably still be too slow when flying, so we'll borrow our animation controller from the first method with some changes to give the entity speed when it's flying.

<CodeHeader></CodeHeader>

```json
"controller.animation.dragon.flying":{
    "states":{
        "default":{
            "transitions":[
                {
                    "jumping_1":"!q.is_on_ground"
                }
            ]
        },
        "jumping_1":{
            "transitions":[
                {
                    "transition_to_default":"q.is_on_ground"
                },
                {
                    "jumping_2":"true"
                }
            ],
            "on_entry":[
                "/effect @s speed 15 10 true"
            ]
        },
        "jumping_2":{
            "transitions":[
                {
                    "transition_to_default":"q.is_on_ground"
                },
                {
                    "jumping_1":"true"
                }
            ],
            "on_entry":[
                "/effect @s speed 15 10 true"
            ]
        },
        "transition_to_default":{
            "transitions":[
                {
                    "transition_to_default":"true"
                }
            ],
            "on_entry":[
                "/effect @s clear"
            ]
        }
    }
}
```

_Since the entity's effects might be cleared when it's being flown, we changed the animation controller to give the entity speed every tick it's not on the ground._

You might also notice that the entity levitates when you go near it. We can fix this by giving the entity a tag when it's being ridden (removing it when it isn't being ridden) and only applying those effects when the entity has the tag by making and animating another animation controller and updating our commands.

<CodeHeader></CodeHeader>

```json
"controller.animation.dragon.test_rider":{
    "states":{
        "default":{
            "transitions":[
                {
                    "has_rider":"q.has_rider"
                }
            ]
        },
        "has_rider":{
            "transitions":[
                {
                    "default":"!q.has_rider"
                }
            ],
            "on_entry":[
                "/tag @s add has_rider"
            ],
            "on_exit":[
                "/tag @s remove has_rider"
            ]
        }
    }
}
```

<CodeHeader></CodeHeader>

```
execute as @a[rxm=-90,rx=-25] run effect @e[type=wiki:dragon,r=1,tag=has_rider] levitation 1 6 true
execute as @a[rxm=-25,rx=-15] run effect @e[type=wiki:dragon,r=1,tag=has_rider] levitation 1 3 true
execute as @a[rxm=-15,rx=-5] run effect @e[type=wiki:dragon,r=1,tag=has_rider] levitation 1 2 true
execute as @a[rxm=-5,rx=20] run effect @e[type=wiki:dragon,r=1,tag=has_rider] levitation 1 1 true
execute as @a[rxm=20,rx=35] run effect @e[type=wiki:dragon,r=1,tag=has_rider] slow_falling 1 1 true
execute as @a[rxm=35,rx=90] run effect @e[type=wiki:dragon,r=1,tag=has_rider] clear
```

## Controlling Through Jumping

A third method of controlling flying entities uses the player's jump button. The entity rises when the player is holding the jump button and falls when they release their jump button.

To do this, we need an animation controller attached to the player rather than the entity itself to detect when the player uses their jump button. We also need to disable dismounting when the player presses the jump button.

First, on the entity, disable dismounting and jumping:

<CodeHeader></CodeHeader>

```json
"minecraft:horse.jump_strength": {
    "value": 0
},
"minecraft:can_power_jump": {}
```

Next, we need an animation controller that causes the entity to levitate when the player uses their jump button and resets the levitation when they release their jump button.

<CodeHeader></CodeHeader>

```json
"controller.animation.fly_dragon":{
    "initial_state":"falling",
    "states":{
        "falling":{
            "on_entry":[
                "/effect @e[type=wiki:dragon,r=1,c=1] levitation 0"
            ],
            "transitions":[
                {
                    "rising":"q.is_jumping"
                }
            ]
        },
        "rising":{
            "on_entry":[
                "/effect @e[type=wiki:dragon,r=1,c=1] levitation 100000 6 true"
            ],
            "transitions":[
                {
                    "falling":"!q.is_jumping"
                }
            ]
        }
    }
}
```

Now, we need a copy of the player's behavior file, which we will modify slightly. You can find the player's behavior file in the vanilla behavior pack provided by Mojang (found [here](https://aka.ms/behaviorpacktemplate)). Once you have copied the player's behavior file to your own behavior pack, find their `"description"` object and add the animation controller. We also want to ensure that the entity will only respond to the player's jump input when the player is riding it, so we can use a Molang query in the player's behavior to only activate the animation controller when the player is riding.

<CodeHeader></CodeHeader>

```json
"description":{
    "identifier":"minecraft:player",
    "is_spawnable":false,
    "is_summonable":false,
    "animations":{
        "fly_dragon":"controller.animation.fly_dragon"
    },
    "scripts":{
        "animate":[
            {
                "fly_dragon":"q.is_riding"
            }
        ]
    }
}
```

The entity can now be controlled with the jump key, but there's a bug. If the player dismounts the entity while holding the jump key, it will continue rising. We can fix this with an animation controller on the entity itself that resets the levitation whenever a player dismounts it.

<CodeHeader></CodeHeader>

```json
"controller.animation.reset_levitation":{
    "initial_state":"no_rider",
    "states":{
        "no_rider":{
            "transitions":[
                {
                    "has_rider":"q.has_rider"
                }
            ]
        },
        "has_rider":{
            "on_exit":[
                "/effect @s levitation 0"
            ],
            "transitions":[
                {
                    "no_rider":"!q.has_rider"
                }
            ]
        }
    }
}
```

## Controlling by Scripts

This fourth method allows us to adjust the falling speed, movement speed and it works when player jumps. It is essential to add the horse jump function so that when the player jumps, he does not fall off the entity, also it's very important to add the family type that indicates that it can fly, since we handle this in the scripts.

<CodeHeader>minecraft:entity</CodeHeader>

```json
"components": {
    "minecraft:behavior.player_ride_tamed": {},
    "minecraft:input_ground_controlled": {},
    "minecraft:can_power_jump": {},
    "minecraft:horse.jump_strength": {
        "value": 0
    },
    "minecraft:damage_sensor": {
        "triggers": [
            {
                "cause": "fall",
                "deals_damage": false
            }
        ]
    },
    "minecraft:rideable": {
        "seat_count": 1,
        "interact_text": "action.interact.mount",
        "family_types": ["player"],
        "seats": {
            "position": [0.0, 0.63, 0.0]
        }
    },
    "minecraft:type_family": {
        "family": ["pig", "mob", "wiki:can_fly"] // Indicates that the entity can fly
    }
}
```

After adjusting our entity with the previous configurations, we are going to add the script to give it functionality.

<CodeHeader>BP/scripts/utils.js</CodeHeader>

```js
import { Entity } from "@minecraft/server";
class Utils {
    /**
     * @param {Entity} entity
     */
    constructor(entity) {
        this.entity = entity;
        this.rideable = entity?.getComponent("rideable");
        this.player = this.rideable?.getRiders()[0];
        this.riding = this.player?.getComponent("riding");
    }

    /**
     * @param {number} flySpeed
     * @param {number} fallSpeed
     * @param {number} XZspeed
     */
    flySystem(flySpeed, fallSpeed, XZspeed) {
        if (!this.riding) return;
        const direction = {
            x: 0,
            y: this.player.isJumping ? flySpeed : fallSpeed,
            z: 0,
        };
        this.entity.addEffect("speed", 5, {
            showParticles: false,
            amplifier: XZspeed,
        });
        this.entity.applyImpulse(direction);
    }
}

export default Utils;
```

The utils.js file creates a function to make the entity able to fly.
Now we need to apply it to our entity for this to work.

<CodeHeader>BP/scripts/index.js</CodeHeader>

```js
import { system, world } from "@minecraft/server";
import Utils from "./utils";

system.runInterval(() => {
    const dim = world.getDimension("overworld");
    // You can use tags instead of family type
    for (const entity of dim.getEntities({ families: ["wiki:can_fly"] })) {
        const utils = new Utils(entity);
        // Recommended values
        utils.flySystem(0.09, 0.07, 5);
    }
});
```

You can adjust the speed from index.js.

-   Speed ​​on Y axis (UP)

    **flySpeed: 0.09**

-   Speed ​​on Y axis (DOWN)

    **fallSpeed: 0.07**

-   Speed on X axis (Horizontal)

    **XZspeed: 5**

These are just recommended values, you're free to change the velocity.
