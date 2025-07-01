---
title: Troubleshooting Entities
category: General
nav_order: 3
tags:
    - help
mentions:
    - SirLich
    - BlueFrog130
    - SmokeyStack
    - MedicalJewel105
    - aexer0e
    - ChibiMango
    - RonarsCorruption
description: Troubleshooting guide for entities.
---

:::tip
This page contains troubleshooting information about _entities_. You should read our [global troubleshooting](/guide/troubleshooting) document before continuing here.
:::

:::warning
Always remember to check content log!
:::

## 0.0.0 - You messed up

Accept that something, somewhere, is wrong. _Nobody_ at _any_ level is immune to these mistakes, so don't get offended and think, "Of course I did that!" and skip a step!

<Button link="#_1-0-0-are-both-packs-active">Continue</Button>

## 1.0.0 - Are both packs active?

Make sure both the resource pack and behavior pack are active for the world (an excellent way to avoid accidentally having this issue is to set each pack as a dependency of the other in both packs' manifest.json files so that adding or removing one of the packs automatically adds/removes the other)

<Button link="#_2-0-0-determine-whether-the-issue-is-in-the-rp-or-the-bp">Continue</Button>

## 2.0.0 - Determine whether the issue is in the RP or the BP

The issue you're suffering can be narrowed down significantly by how your entity's spawn egg appears in the creative inventory. Even if you don't want the entity to have a spawn egg, make the following changes just for now until you locate the issue:

### In the RP

Make sure the .entity file has a custom spawn_egg object like:

<CodeHeader></CodeHeader>

```json
"spawn_egg":{
    "base_color": "#FF0000",
    "overlay_color": "#FFFF00"
}
```

The colors you choose will need to be something other than "#000000" for this guide.

### In the BP

Make sure `is_spawnable` and `is_summonable` are set to true, and that `is_experimental` is set to `false` in the description object:

<CodeHeader></CodeHeader>

```json
"description":{
    "identifier": "wiki:example_entity",
    "is_spawnable": true,
    "is_summonable": true,
    "is_experimental": false
}
```

### Results

I don't see a spawn egg at all: <Button link="#_3-1-0-bp">Go</Button>

I see a spawn egg for my entity, but it's just black, and the entity doesn't appear when I spawn or summon it: <Button link="#step-3-2-0-rp-entity">Go</Button>

I see a spawn egg for my entity, and it has the colors I chose, but the entity still doesn't appear when I spawn or summon it: <Button link="#step-3-3-0-rp-resources-still-writing-because-this-is-going-to-be-extensive">Go</Button>

## 3.0.0 - Locating the specific issue

## 3.1.0 - BP

_You don't see a spawn egg for your entity in the creative inventory, even after making sure "is_spawnable" is set to true in the behavior file._

This means the game isn't detecting a behavior file for the entity at all. Some common reasons for this include:

-   Syntax error in your behavior file
-   Misnamed folder

### 3.1.1 - Syntax error

A single syntax error in a .json file causes the entire file to break and be ignored. To check that your file is free of syntax errors, visit [Json Lint](https://jsonlint.com/), paste the contents of your behavior file into the big box, then click "Validate JSON".
(NOTE: Although this site will mark // comments as errors, Minecraft DOES allow .json files to contain them)

### 3.1.2 - Misnamed folder

Ensure the folder containing your behavior files is named "entities" and not "entity". In behavior packs, folders tend to be named "entities" while in resource packs, they'll usually be "entity". I know. It isn't enjoyable.

## Step 3.2.0 - RP .entity

_You DO see a spawn egg for your entity in the creative inventory, but it's black (and probably has a weird name like "item.spawn_egg.entity.wiki:your_mob.name"), and nothing appears when you spawn/summon it._

This means you have a working behavior file, but for whatever reason, the game isn't connecting it to the corresponding .entity file in your resource pack. Some common reasons for this include:

-   Syntax error in your .entity file
-   The entity's identifiers don't match
-   One or more of the resources your .entity file directs to are invalid
-   Check that your RP folder is "entity", and your BP folder is "entities"

### Step 3.2.1 - Syntax error

A single syntax error in a .json file causes the entire file to break and be ignored. To check that your file is free of syntax errors, visit [Json Lint](https://jsonlint.com/), paste the contents of your behavior file into the big box, then click "Validate JSON".
(NOTE: Although this site will mark // comments as errors, Minecraft DOES allow .json files to contain them)

### Step 3.2.2 - Identifiers don't match

The "identifier" in your behavior file must be EXACTLY the same as the one in your .entity file, including the namespace (the part before the colon like `minecraft` in `minecraft:bat`), and neither should be using `minecraft` as the namespace unless it's a default mob.

Your identifiers should also NOT contain any spaces or special characters (aside from the colon between the namespace and ID), and, for rare fringe case bug reasons, you should AVOID having the namespace or ID start with anything other than a lowercase letter. Beginning with a number or capital letter _shouldn't_ be an issue anymore, but this was not always the case in earlier versions of the game, and because of this, bugs have sporadically appeared in the past where starting with a number or capital letter had unexpected effects. Therefore it's better to avoid this if possible.

### Step 3.2.3 - Invalid resources

The entity's ID in the .entity file does not match the ID you used in the behavior file.

## Step 3.3.0 - RP resources: (WIP)

_You DO see a spawn egg for your entity in the creative inventory, and it DOES have the proper colors you specified in the .entity file's "spawn_egg" object, but nothing appears when you spawn/summon it, or there's just a shadow._

This means you have a working `.behavior` and `.entity` file, but something in the `.entity` file directs to either a broken file or another valid file that leads to a broken file.

To start with:

-   invisible, no shadow -> bad RP reference: <Button link="#_3-3-1-invisible-no-shadow">Go</Button>
-   invisible, shadow exists -> geometry issue: <Button link="#_3-3-2-invisible-shadow-exists">Go</Button>
-   visible, weird texture -> texture issue: <Button link="#_3-3-3-visible-weird-texture">Go</Button>
-   visible, weird visibility stuff -> material issue: <Button link="#_3-3-4-visible-weird-visibility-stuff">Go</Button>

### 3.3.1 - Invisible, no shadow

This can be caused by ... . First make sure your entity is in it's place (not disappearing, for example it does not do instant_despawn).

### 3.3.2 - Invisible, shadow exists

This situation can be caused by wrong geometry or wrong material (if using half-transparent a.k.a glowing textures).

1. Make sure you don't have a spelling error in geometry name, geometry file is valid and geometry offsets are correct.
2. Make sure you are using the right material. For example, some materials support only emissive textures.
3. Check your render controllers. Maybe the issue is in it.

### 3.3.3 - Visible, weird texture

### 3.3.4 - Visible, weird visibility stuff
