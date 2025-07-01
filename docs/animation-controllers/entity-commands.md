---
title: Entity Commands
nav_order: 2
tags:
    - intermediate
mentions:
    - SirLich
    - solvedDev
    - Joelant05
    - destruc7i0n
    - Dreamedc2015
    - MedicalJewel105
    - aexer0e
    - cda94581
    - ThijsHankelMC
    - QuazChick
description: Trigger slash commands from entities.
---

:::tip EVENT RESPONSE
A much easier method of running entity commands is through the `queue_command` entity event response.
:::

## Animation Controllers

To trigger slash commands, we are going to use Behavior Pack animation controllers. Animation controllers should be placed like: `animation_controllers/some_controller.json`. You can [learn more about animation controllers on the entity events section of bedrock.dev](https://bedrock.dev/docs/stable/Entity%20Events).

In short, animation controllers allow us to trigger events from behavior packs.

-   Slash commands (like `/say`)
-   Molang ( `v.foo += 1;` )
-   Entity Events (such as `@s wiki:my_event`)

Here is an example animation controller:

<CodeHeader>BP/animation_controllers/entity_commands.ac.json</CodeHeader>

```json
{
    "format_version": "1.10.0",
    "animation_controllers": {
        "controller.animation.sirlich_entity_commands": {
            "states": {
                "default": {
                    "transitions": [
                        {
                            "on_summon": "1" //1 evaluates as true
                        }
                    ]
                },
                "on_summon": {
                    "on_entry": ["/say I have been summoned"]
                }
            }
        }
    }
}
```

This animation controller will run the command `/say I have been summoned` as soon as the entity is summoned into the world. If you are confused about how this works, please review Molang, Animations, and Entity Events.

In short, there are `states`, which can trigger events in their `on_entry` clause. We use queries to move between different states. By default, entities will be inside of the `default` state, unless an `initial_state` value has been defined.

::: warning
Queries are re-run when the world/chunk reloads. This means the line `"/say I have been summoned"` will actually run each time the entity "loads" - not only when it is summoned.
:::

If you need to stop this from happening, you need to add additional queries, such as a `skin_id` query. The first time the entity spawns, check for `skin_id = 0`, and then _also_ add some higher `skin_id`, such as `skin_id = 1`. Then, when the entity reloads, it won't be able to run those commands. This is shown further down in the document.

## Using Animation Controllers

To add this animation controller to our entity, we can use the following code in the entity definition description:

<CodeHeader>BP/entities/entity_commands.se.json</CodeHeader>

```json
"description": {
    "identifier": "wiki:entity_commands",
    "scripts": {
        "animate": [
            "wiki:entity_commands"
        ]
    },
    "animations": {
        "wiki:entity_commands": "controller.animation.wiki_entity_commands"
    }
}
```

Once again, if you are confused about any of this step, please review the [Entity Events documentation](https://bedrock.dev/r/Entity%20Events).

## Triggering Commands using Events:

Animation transitions are created using queries. You can read about queries [here](https://bedrock.dev/docs/stable/MoLang#List%20of%20Entity%20Queries). In our first example, our query was simply `true`, which means the commands run automatically. We can use more complicated queries to create more interesting effect. A really convenient method is using components as Molang filters to trigger the commands.

I personally like using [skin_id](https://docs.microsoft.com/en-us/minecraft/creator/reference/content/entityreference/examples/entityproperties/minecraftproperty_skin_id).

We can update our animation controller to trigger based on `skin_id`:

<CodeHeader>BP/animation_controllers/entity_commands.ac.json</CodeHeader>

```json
{
    "format_version": "1.10.0",
    "animation_controllers": {
        "controller.animation.sirlich_entity_commands": {
            "states": {
                "default": {
                    "transitions": [
                        {
                            "command_example": "q.skin_id == 1"
                        },
                        {
                            "zombies": "q.skin_id == 2"
                        }
                    ]
                },
                "command_example": {
                    "transitions": [
                        {
                            "default": "q.skin_id != 1"
                        }
                    ],
                    "on_entry": ["/say Command One!", "@s execute_no_commands"]
                },
                "zombies": {
                    "transitions": [
                        {
                            "default": "q.skin_id != 2"
                        }
                    ],
                    "on_entry": [
                        "/say AHH! Zombies everywhere!",
                        "/summon minecraft:zombie",
                        "/summon minecraft:zombie",
                        "/summon minecraft:zombie",
                        "/summon minecraft:zombie",
                        "@s execute_no_commands"
                    ]
                }
            }
        }
    }
}
```

This animation controller has two command states now: The first is triggered by `skin_id = 1`, and the second by `skin_id = 2`. Notice that `==` and `!=` was used. `==` tests for equality, do NOT use a single `=`. `!` means NOT, so `!=` tests to make sure it is NOT equal to a specific value. Additionally, note how I've added the `@s execute_no_commands` syntax at the end of each command list. We will create `execute_no_commands` later. It will allow us to set the skin_id back to 0, and re-use our commands.

The syntax is `@s` followed by the name of an entity event. This allows us to add/remove components from within the animation controller.

## Setting Component Groups

Back in our entity file, we can set the `skin_id` using the `skin_id` component.

The `skin_id` component looks like this:

<CodeHeader></CodeHeader>

```json
"minecraft:skin_id": {
    "value": 1
}
```

We can add component groups that contains skin_ids:

<CodeHeader>BP/entities/entity_commands.se.json</CodeHeader>

```json
"component_groups": {
    "execute_no_commands": {
        "minecraft:skin_id": {
            "value": 0
        }
    },
    "command_example": {
        "minecraft:skin_id": {
            "value": 1
        }
    },
    "command_zombies": {
        "minecraft:skin_id": {
            "value": 2
        }
    }
}
```

## Adding Events

Now let's create events so we can easily add these groups:

<CodeHeader>BP/entities/entity_commands.se.json</CodeHeader>

```json
"events": {
    "minecraft:entity_spawned": {
        "add": {
            "component_groups": [
                "execute_no_commands"
            ]
        }
    },
    "execute_no_commands": {
        "add": {
            "component_groups": [
                "execute_no_commands"
            ]
        }
    },
    "command_example": {
        "add": {
            "component_groups": [
                "command_example"
            ]
        }
    },
    "command_zombies": {
        "add": {
            "component_groups": [
                "command_zombies"
            ]
        }
    }
}
```

## Triggering Events

There are loads of ways to trigger events in Minecraft. As stated earlier, you can use animation controllers to trigger events. Additionally, let's look at two specific examples:

### Interact Component:

This component will spawn zombies whenever you click on him.

<CodeHeader>BP/entities/entity_commands.se.json</CodeHeader>

```json
"minecraft:interact": {
    "interactions": [{
        "on_interact": {
            "filters": {
                "all_of": [{
                        "test": "is_family",
                        "subject": "other",
                        "value": "player"
                    }
                ]
            },
            "event": "command_zombies"
        }
    }]
}
```

### Timer

This component will trigger the example command every 10 seconds:

<CodeHeader>BP/entities/entity_commands.se.json</CodeHeader>

```json
"minecraft:timer": {
    "looping": true,
    "time": 10,
    "time_down_event": {
        "event": "example_command"
    }
}
```

By adding these (and similar!) components to our entity, we can control when the `skin_id` changes, and therefor which events run.

## Review:

Here is how it all works:

-   Run `example_command` using a component like interact or timer.
-   This adds the `example_command` component group
-   This adds the `skin_id` component
-   This sets the entities `skin_id`, which can be queried in the animation controller
-   The animation controller notices this `skin_id`, and moves to the `example_command` state
-   The animation controller runs the `/say` command
-   The animation controller runs the entity event `@s execute_no_command`
-   `execute_no_command` event sets the `skin_id` to 0
-   The animation controllers sees this, and transitions to the default state
-   Now the animation controller waits for a new `skin_id`
