---
title: Death Commands
mentions:
    - SirLich
    - BlueFrog130
    - SmokeyStack
    - cda94581
    - MedicalJewel105
    - Kaioga5
    - TheItsNameless
    - QuazChick
description: Run command when entity dies.
---

<Button link="animation-controllers-intro">Learn more about Animation Controllers</Button>

I define `Death Effects` as "Doing something when an Entity dies". There are a few wrong ways to achieve this that should be avoided, including:

-   Detecting death in the entity file, adding a component, and _then_ trying to detect that component in the animation controller. This is wrong because the entity will be removed from the world before the animation controller has a chance to run.
-   Detecting the entity death from an outside source, such as a ticking command block. This method isn't _strictly_ wrong, and in some circumstances, it may even be preferred. However it is costly and easy to break.

## Using q.is_alive

The best way to create death effects is by using the `is_alive` query.

Simply create an animation controller with a transition based on `is_alive`. The final `on_entry` will run before the entity is removed from the world, allowing you to run your commands.

Here is a sample animation controller:

<CodeHeader>BP/animation_controllers/death.ac.json</CodeHeader>

```json
{
    "format_version": "1.10.0",
    "animation_controllers": {
        "controller.animation.death": {
            "initial_state": "default",
            "states": {
                "default": {
                    "transitions": [
                        {
                            "dead": "!q.is_alive"
                        }
                    ]
                },
                "dead": {
                    "on_entry": ["/say I am dead!"]
                }
            }
        }
    }
}
```

## Use on player entities

In the case of player entities, an additional transition must be added to the second animation state in order to ensure the state resets between deaths:

<CodeHeader>BP/animation_controllers/death.ac.json</CodeHeader>

```json
{
    "format_version": "1.10.0",
    "animation_controllers": {
        "controller.animation.death": {
            "initial_state": "default",
            "states": {
                "default": {
                    "transitions": [
                        {
                            "dead": "!q.is_alive"
                        }
                    ]
                },
                "dead": {
                    "on_entry": ["/say I am dead!"],
                    "transitions": [
                        {
                            "default": "q.is_alive"
                        }
                    ]
                }
            }
        }
    }
}
```

## Using minecraft:on_death

You can also use the `minecraft:on_death` component in your `entity.json` file in the Behavior Pack, which is a fairly easy way to accomplish a command on death.

You first add it to your components and make it run an event on self;

```json
"minecraft:on_death" : {
    "event": "wiki:on_death",
    "target": "self"
}
```

And then, in your events section you add the event;

```json
"wiki:on_death": {
    "queue_command": {
        "command": [
            "say I have died!"
        ]
    }
}
```

:::tip
You can add scores and tags to the entity even when it is dead using this method.
:::
