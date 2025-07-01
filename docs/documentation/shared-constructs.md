---
title: Shared Constructs
description: JSON constructs are expressible in multiple locations in the add-ons system.
nav_order: 1
mentions:
    - Ciosciaa
    - ThomasOrs
    - QuazChick
---

JSON constructs are expressible in multiple locations in the add-ons system.

## Block Descriptors

Block descriptors can be used to check for blocks that match the specified properties.
For example, block descriptors are used in the block [placement filter](/blocks/block-components#placement-filter) to determine which types of blocks a custom block can survive on.

<CodeHeader>Identifier Object Example</CodeHeader>

```json
{
    "name": "wiki:my_block"
}
```

<CodeHeader>Permutation Object Example</CodeHeader>

```json
{
    "name": "wiki:my_block",
    "states": {
        "wiki:my_state": 5
    }
}
```

<CodeHeader>Tag Object Example</CodeHeader>

```json
{
    "tags": "q.any_tag('minecraft:is_axe_item_destructible', 'wiki:my_tag')"
}
```

## Range Objects

Range objects define a spread between two numbers.

<CodeHeader>Range Object Example</CodeHeader>

```json
{
    "min": 2,
    "max": 4
}
```

When provided, a random value will be selected inclusively between the minimum and maximum. Rolls are not retained; a new random value will be rolled each instance the range object would be used. The maximum must not be less than the minimum, but they may be equal to affix rolls to a specific value.

## Fraction Objects

Fraction objects define a fraction using a numerator and denominator.

<CodeHeader>Fraction Object Example</CodeHeader>

```json
{
    "numerator": 3,
    "denominator": 5
}
```

The value used in place of the object will be the computed division, `numerator` ÷ `denominator`. Both the numerator and denominator must be at least `1`, and the denominator cannot be equal to the numerator.
