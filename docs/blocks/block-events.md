---
title: Block Events
description: Block events trigger when certain conditions are met. Creators can hook into these events to modify the game world when events are triggered.
category: General
nav_order: 8
tags:
    - scripting
license: true
mentions:
    - SirLich
    - solvedDev
    - yanasakana
    - MedicalJewel105
    - aexer0e
    - SmokeyStack
    - TheDoctor15
    - XxPoggyisLitxX
    - TheItsNameless
    - ThomasOrs
    - QuazChick
    - VactricaKing
    - BlazeDrake
---

:::tip FORMAT VERSION `1.21.90`
Using the latest format version when creating custom blocks provides access to fresh features and improvements.
The wiki aims to share up-to-date information about custom blocks, and currently targets format version `1.21.90`.
:::

## Registering Custom Components

Block events trigger when certain conditions are met and can be "listened" to in **custom components** which are registered in scripts before the world is loaded.

Within each custom component, event handler functions (such as [`beforeOnPlayerPlace`](#before-player-place)) are listed to configure what you want to happen when each event is triggered.

_This example prevents the player from placing the block if they aren't in creative mode:_

<CodeHeader>BP/scripts/creativeModeOnly.js</CodeHeader>

```js
import { system, GameMode } from "@minecraft/server";

/** @type {import("@minecraft/server").BlockCustomComponent} */
const BlockCreativeModeOnlyComponent = {
    beforeOnPlayerPlace({ player }) {
        const gameMode = player?.getGameMode();

        if (gameMode === GameMode.Creative) {
            event.cancel = true;
        }
    },
};

system.beforeEvents.startup.subscribe(({ blockComponentRegistry }) => {
    blockComponentRegistry.registerCustomComponent(
        "wiki:creative_mode_only",
        BlockCreativeModeOnlyComponent
    );
});
```

## Applying Custom Components

To bind a custom component to a block, simply list it in the `components` of your block JSON.

Like any normal component, custom components can be added and removed based on the block's [permutation](/blocks/block-permutations).

<CodeHeader>minecraft:block</CodeHeader>

```json
"components": {
    "wiki:creative_mode_only": {}
}
```

## List of Events

### Before Player Place

Runs before a player places the block, preventing the client-side placement of the block.

<CodeHeader>Custom Component</CodeHeader>

```js
beforeOnPlayerPlace(event) {
    event.block // Block impacted by this event. This is the block that will be replaced.
    event.cancel // If set to true, cancels the block place event.
    event.dimension // Dimension that contains the block.
    event.face // The block face that was placed onto.
    event.permutationToPlace // The block permutation that will be placed. Can be changed to place a different permutation instead.
    event.player // The player that is placing this block. May be undefined.
}
```

### Entity Fall On

:::tip DEPENDENCIES
The entity fall on event requires the [`minecraft:entity_fall_on`](/blocks/block-components#entity-fall-on) component to be active on your block to trigger.

The entity fall on event requires the [`minecraft:collision_box`](/blocks/block-components#collision-box) component to be 4 or higher on the Y-axis in order to trigger.
:::

Runs when an entity falls on the block.

<CodeHeader>minecraft:block > components</CodeHeader>

```json
"minecraft:entity_fall_on": {
    "min_fall_distance": 5 // The minimum distance an entity must fall to trigger this event (optional).
}
```

<CodeHeader>Custom Component</CodeHeader>

```js
onEntityFallOn(event) {
    event.block // Block impacted by this event.
    event.dimension // Dimension that contains the block.
    event.entity // The entity that stepped on the block. May be undefined.
    event.fallDistance // The distance that the entity fell before landing.
}
```

### Place

Runs when the block is placed.

<CodeHeader>Custom Component</CodeHeader>

```js
onPlace(event) {
    event.block // Block impacted by this event.
    event.dimension // Dimension that contains the block.
    event.previousBlock // Permutation of the replaced block.
}
```

### Player Break

Runs when the player breaks the block.

<CodeHeader>Custom Component</CodeHeader>

```js
onPlayerBreak(event) {
    event.block // Block impacted by this event. This is the block after it has been broken.
    event.brokenBlockPermutation // Permutation of the block before it was broken.
    event.dimension // Dimension that contains the block.
    event.player // The player that broke the block. May be undefined.
}
```

### Player Interact

Runs when the player interacts with / uses the block.

<CodeHeader>Custom Component</CodeHeader>

```js
onPlayerInteract(event) {
    event.block // Block impacted by this event.
    event.dimension // Dimension that contains the block.
    event.face // The block face that was interacted with.
    event.faceLocation // Location relative to the bottom north-west corner of the block that the player interacted with.
    event.player // The player that interacted with the block. May be undefined.
}
```

### Random Tick

Triggered on every random tick, allowing for behavior like random crop growth.

<CodeHeader>Custom Component</CodeHeader>

```js
onRandomTick(event) {
    event.block // Block impacted by this event.
    event.dimension // Dimension that contains the block.
}
```

### Step Off

:::tip DEPENDENCY
The step off event requires the [`minecraft:collision_box`](/blocks/block-components#collision-box) component to be 4 or higher on the Y-axis in order to trigger.
:::

Runs when an entity steps off the block.

<CodeHeader>Custom Component</CodeHeader>

```js
onStepOff(event) {
    event.block // Block impacted by this event.
    event.dimension // Dimension that contains the block.
    event.entity // The entity that stepped on the block. May be undefined.
}
```

### Step On

:::tip DEPENDENCY
The step on event requires the [`minecraft:collision_box`](/blocks/block-components#collision-box) component to be 4 or higher on the Y-axis in order to trigger.
:::

Runs when an entity steps onto the block.

<CodeHeader>Custom Component</CodeHeader>

```js
onStepOn(event) {
    event.block // Block impacted by this event.
    event.dimension // Dimension that contains the block.
    event.entity // The entity that stepped on the block. May be undefined.
}
```

### Tick

:::tip DEPENDENCY
The tick event requires the [`minecraft:tick`](/blocks/block-components#tick) component to be active on your block to trigger.
:::

Triggers between X and Y amount of ticks inside `interval_range` of the block's [`minecraft:tick`](/blocks/block-components#tick) component.

<CodeHeader>minecraft:block > components</CodeHeader>

```json
"minecraft:tick": {
    "interval_range": [10, 20],
    "looping": true
}
```

<CodeHeader>Custom Component</CodeHeader>

```js
onTick(event) {
    event.block // Block impacted by this event.
    event.dimension // Dimension that contains the block.
}
```
