---
title: Vanilla Particles
category: Documentation
description: List of vanilla particles.
---

Here is the complete list of Bedrock particles from the vanilla resources. Please be aware that not all of these particles function properly, as many require molang context from their host entity.

:::tip Playing via /particle:
For some reason, Bedrock requires the leading `minecraft` namespace, and the coordinates in the `/particle` command.

It doesn't have autocomplete for particles.
:::

### Working Particles

These particles can be spawned directly in-world, without any issues.

| Working Particles                           |
|---------------------------------------------|
| minecraft:basic_flame_particle              |
| minecraft:basic_portal_particle             |
| minecraft:basic_smoke_particle              |
| minecraft:bleach                            |
| minecraft:blue_flame_particle               |
| minecraft:camera_shoot_explosion            |
| minecraft:campfire_smoke_particle           |
| minecraft:campfire_tall_smoke_particle      |
| minecraft:candle_flame_particle             |
| minecraft:critical_hit_emitter              |
| minecraft:crop_growth_emitter               |
| minecraft:dragon_breath_trail               |
| minecraft:dragon_death_explosion_emitter    |
| minecraft:dragon_destroy_block              |
| minecraft:dragon_dying_explosion            |
| minecraft:endrod                            |
| minecraft:end_chest                         |
| minecraft:evocation_fang_particle           |
| minecraft:evoker_spell                      |
| minecraft:cauldron_explosion_emitter        |
| minecraft:egg_destroy_emitter               |
| minecraft:falling_border_dust_particle      |
| minecraft:falling_dust_dragon_egg_particle  |
| minecraft:falling_dust_gravel_particle      |
| minecraft:falling_dust_red_sand_particle    |
| minecraft:falling_dust_sand_particle        |
| minecraft:falling_dust_scaffolding_particle |
| minecraft:falling_dust_top_snow_particle    |
| minecraft:heart_particle                    |
| minecraft:honey_drip_particle               |
| minecraft:huge_explosion_lab_misc_emitter   |
| minecraft:huge_explosion_emitter            |
| minecraft:ice_evaporation_emitter           |
| minecraft:knockback_roar_particle           |
| minecraft:lab_table_misc_mystical_particle  |
| minecraft:large_explosion                   |
| minecraft:lava_drip_particle                |
| minecraft:lava_particle                     |
| minecraft:llama_spit_smoke                  |
| minecraft:magnesium_salts_emitter           |
| minecraft:mob_portal                        |
| minecraft:mycelium_dust_particle            |
| minecraft:obsidian_glow_dust_particle       |
| minecraft:obsidian_tear_particle            |
| minecraft:redstone_ore_dust_particle        |
| minecraft:redstone_repeater_dust_particle   |
| minecraft:redstone_torch_dust_particle      |
| minecraft:redstone_wire_dust_particle       |
| minecraft:rising_border_dust_particle       |
| minecraft:sculk_sensor_redstone_particle    |
| minecraft:snowflake_particle                |
| minecraft:spore_blossom_ambient_particle    |
| minecraft:spore_blossom_shower_particle     |
| minecraft:stalactite_lava_drip_particle     |
| minecraft:stalactite_water_drip_particle    |
| minecraft:totem_particle                    |
| minecraft:villager_angry                    |
| minecraft:villager_happy                    |
| minecraft:water_drip_particle               |
| minecraft:water_evaporation_bucket_emitter  |
| minecraft:water_splash_particle_manual      |

### Particles with Issues

The following particles can be spawned, but might spam you with content log errors because they rely on variables that `/particle` cannot set:

| Particles with issues                           |
|-------------------------------------------------|
| minecraft:arrow_spell_emitter                   |
| minecraft:balloon_gas_particle                  |
| minecraft:basic_crit_particle                   |
| minecraft:conduit_absorb_particle               |
| minecraft:conduit_attack_emitter                |
| minecraft:dragon_breath_fire                    |
| minecraft:dragon_breath_lingering               |
| minecraft:electric_spark_particle               |
| minecraft:enchanting_table_particle             |
| minecraft:elephant_tooth_paste_vapor_particle   |
| minecraft:death_explosion_emitter               |
| minecraft:eyeofender_death_explode_particle     |
| minecraft:explosion_particle                    |
| minecraft:falling_dust_concrete_powder_particle |
| minecraft:lab_table_heatblock_dust_particle     |
| minecraft:misc_fire_vapor_particle              |
| minecraft:mobspell_emitter                      |
| minecraft:mob_block_spawn_emitter               |
| minecraft:note_particle                         |
| minecraft:portal_directional                    |
| minecraft:portal_reverse_particle               |
| minecraft:rain_splash_particle                  |
| minecraft:shulker_bullet                        |
| minecraft:silverfish_grief_emitter              |
| minecraft:soul_particle                         |
| minecraft:sparkler_emitter                      |
| minecraft:splash_spell_emitter                  |
| minecraft:water_evaporation_actor_emitter       |
| minecraft:water_splash_particle                 |
| minecraft:water_wake_particle                   |
| minecraft:wax_particle                          |
| minecraft:wither_boss_invulnerable              |

### Bubble Particles

The following particles are various bubbles that only show up underwater. Some of them spam content log errors:

| Bubble particles                       |
|----------------------------------------|
| minecraft:basic_bubble_particle        |
| minecraft:basic_bubble_particle_manual |
| minecraft:bubble_column_bubble         |
| minecraft:bubble_column_down_particle  |
| minecraft:bubble_column_up_particle    |
| minecraft:cauldron_bubble_particle     |
| minecraft:cauldron_splash_particle     |
| minecraft:dolphin_move_particle        |
| minecraft:eye_of_ender_bubble_particle |
| minecraft:fish_hook_particle           |
| minecraft:fish_pos_particle            |
| minecraft:glow_particle                |
| minecraft:guardian_attack_particle     |
| minecraft:guardian_water_move_particle |
| minecraft:sponge_absorb_water_particle |
| minecraft:squid_flee_particle          |
| minecraft:squid_ink_bubble             |
| minecraft:squid_move_particle          |
| minecraft:underwater_torch_particle    |

### Permanent Particles

The following particles are permanent and will not be removed once spawned until you exit the game:

| Permanent particles              |
|----------------------------------|
| minecraft:mobflame_emitter       |
| minecraft:nectar_drip_particle   |
| minecraft:phantom_trail_particle |
| minecraft:stunned_emitter        |

## Broken Particles

The following particles exist in-game but cannot be spawned because they require context that cannot be provided by `/particle` or are simply bugged:

| Broken particles                 |
|----------------------------------|
| minecraft:block_destruct         |
| minecraft:block_slide            |
| minecraft:breaking_item_icon     |
| minecraft:breaking_item_terrain  |
| minecraft:cauldron_spell_emitter |
| minecraft:ink_emitter            |
| minecraft:portal_east_west       |
| minecraft:portal_north_south     |
| minecraft:vibration_signal       |
| minecraft:colored_flame_particle |

---

[Original Credit](https://www.reddit.com/r/MinecraftCommands/comments/cbd56p/i_need_a_list_of_bedrock_particles/etg8rt7/)

## Component Particles

The following is a list of pre-defined short names for Vanilla particles that can be used in certain components.
**These have all been proven to generally work. This may not be a full list.**

| Short names           | Notes                                                      |
|-----------------------|------------------------------------------------------------|
| mobspellambient       | Color determined by any present potion ID in the component |
| villagerangry         |                                                            |
| bubble                | Only shows underwater                                      |
| evaporation           |                                                            |
| crit                  |                                                            |
| dragonbreath          | Only seems to work for the AoE component for Projectiles   |
| driplava              |                                                            |
| dripwater             |                                                            |
| reddust               |                                                            |
| enchantingtable       |                                                            |
| endrod                |                                                            |
| mobspell              | Color determined by any present potion ID in the component |
| largeexplode          |                                                            |
| hugeexplosion         |                                                            |
| fallingdust           | Color determined by any present potion ID in the component |
| waterwake             |                                                            |
| flame                 |                                                            |
| villagerhappy         |                                                            |
| heart                 |                                                            |
| mobspellinstantaneous | Color determined by any present potion ID in the component |
| iconcrack             |                                                            |
| slime                 |                                                            |
| snowballpoof          |                                                            |
| largesmoke            |                                                            |
| lava                  |                                                            |
| mobflame              |                                                            |
| townaura              |                                                            |
| note                  |                                                            |
| explode               |                                                            |
| portal                |                                                            |
| rainsplash            |                                                            |
| smoke                 |                                                            |
| watersplash           |                                                            |
| ink                   |                                                            |
| terrain               | Pulls texture from atlas.terrain                           |
| totem                 |                                                            |
| witchspell            |                                                            |
| soul                  |                                                            |
| spit                  |                                                            |
| sneeze                |                                                            |
