---
title: Item Events
description: Item events trigger when certain conditions are met. Creators can hook into these events to modify the game world when events are triggered.
category: General
nav_order: 4
tags:
    - scripting
license: true
mentions:
    - SmokeyStack
---

:::tip FORMAT VERSION `1.21.90`
Using the latest format version when creating custom items provides access to fresh features and improvements. The wiki aims to share up-to-date information about custom items, and currently targets format version `1.21.90`.
:::

## Registering Custom Components

Item events trigger when certain conditions are met and can be "listened" to in **custom components** which are registered in scripts before the world is loaded.

Within each custom component, event handler functions (such as [`onBeforeDurabilityDamage`](#onBeforeDurabilityDamage)) are listed to configure what you want to happen when each event is triggered.

_This example prevents the item from taking durability damage when hitting an entity:_

<CodeHeader>BP/scripts/unbreakable_component.js</CodeHeader>

```js
import { world } from "@minecraft/server";

const ItemUnbreakableComponent = {
    onBeforeDurabilityDamage(event) {
        event.durabilityDamage = 0;
    },
};

world.beforeEvents.worldInitialize.subscribe(({ itemComponentRegistry }) => {
    itemComponentRegistry.registerCustomComponent("wiki:unbreakable", ItemUnbreakableComponent);
});
```

## Applying Custom Components

To bind a custom component to a custom item, simply list them in the [`minecraft:custom_components`](/items/item-components#custom-components) component in your item JSON.

<CodeHeader>minecraft:item</CodeHeader>

```json
"components": {
    "minecraft:custom_components": ["wiki:unbreakable"]
}
```

## List of Events

### Before Durability Damage

This event will be called when an item containing this component is hitting an entity and about to take durability damage.

<CodeHeader>Custom Component</CodeHeader>

```js
onBeforeDurabilityDamage(event) {
    event.attackingEntity // The attacking entity.
    event.durabilityDamage // The damage applied to the item's durability when the event occurs.
    event.hitEntity // The entity being hit.
    event.itemStack // The item stack used to hit the entity.
}
```

### Complete Use

:::tip DEPENDENCY
The complete use event requires the [`minecraft:use_modifiers`](/items/item-components#use-modifiers) component to be active on your item to trigger.
:::

This event will be called when the use duration of the item containing this component is completed.

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:use_modifiers": {
    "use_duration": 5
}
```

<CodeHeader>Custom Component</CodeHeader>

```js
onCompleteUse(event) {
    event.itemStack // Returns the item stack that has completed charging.
    event.source // Returns the source entity that triggered this item event.
}
```

### Consume

This event will be called when an item containing this component is eaten by an entity.

:::tip DEPENDENCIES
The complete use event requires the [`minecraft:use_modifiers`](/items/item-components#use-modifiers) and the [`minecraft:food`](/items/item-components#food) component to be active on your item to trigger.
:::

<CodeHeader>minecraft:item > components</CodeHeader>

```json
"minecraft:food": {},
"minecraft:use_modifiers": {
    "use_duration": 5
}
```

<CodeHeader>Custom Component</CodeHeader>

```js
onConsume(event) {
    event.itemStack // The item stack that was consumed.
    event.source // The source entity that consumed the item.
}
```

### Hit Entity

This function will be called when an item containing this component is used to hit another entity.

<CodeHeader>Custom Component</CodeHeader>

```js
onHitEntity(event) {
    event.attackingEntity // The attacking entity.
    event.hadEffect // Whether the hit landed or had any effect.
    event.hitEntity // The entity being hit.
    event.itemStack // The item stack used to hit the entity.
}
```

### Mine Block

This function will be called when an item containing this component is used to mine a block.

<CodeHeader>Custom Component</CodeHeader>

```js
onMineBlock(event) {
    event.block // The block impacted by this event.
    event.itemStack // The item stack used to mine the block.
    event.minedBlockPermutation // The block permutation that was mined.
    event.source // The entity that mined the block.
}
```

### Use

This function will be called when an item containing this component is used by a player.

<CodeHeader>Custom Component</CodeHeader>

```js
onUse(event) {
    event.itemStack // The item stack when the item was used.
    event.source // The player who used the item.
}
```

### Use On

This function will be called when an item containing this component is used on a block.

<CodeHeader>Custom Component</CodeHeader>

```js
onUseOn(event) {
    event.source // The entity that used the item on the block.
    event.usedOnBlockPermutation // The block permutation that the item was used on.
}
```
