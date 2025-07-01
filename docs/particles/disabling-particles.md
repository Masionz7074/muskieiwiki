---
title: Disabling Particles
description: Remove vanilla particles from displaying in-game.
category: Tutorials
show_outline: false
tags:
    - beginner
mentions:
    - SirLich
    - Joelant05
    - MedicalJewel105
---

In the event that you want to disable a particle, it is recommended to do so from the particle file itself as opposed to simply making the particle texture transparent in `particles.png`. Additionally, disabling a particle might offer a slight performance boost compared to making it transparent, as transparent particles are still emitted (but not visible).

The basic idea of disabling a particle from emitting is as follows:

<CodeHeader>RP/particles/some_vanilla_particle.json</CodeHeader>

```json
{
    "format_version": "1.10.0",
    "particle_effect": {
        "description": {
            "identifier": "minecraft:some_vanilla_particle",
            "basic_render_parameters": {
                "material": "particles_alpha",
                "texture": "textures/particle/particles"
            }
        },
        "components": {
            "minecraft:emitter_lifetime_expression": {
                "activation_expression": 0,
                "expiration_expression": 1
            },
            "minecraft:emitter_rate_manual": {
                "max_particles": 0
            }
        }
    }
}
```
