---
title: Avoiding State Limit
description: Blocks have a limit of 16 valid values per state that cannot be exceeded. This guide will explain how to avoid reaching the limit.
category: Tutorials
tags:
    - expert
mentions:
    - Kaioga5
    - QuazChick
---

Blocks have a limit of 16 valid values per state that cannot be exceeded.
This guide will explain how to avoid reaching the limit.

:::tip
This tutorial does not show you how to have more than 16 values for a single state, however using this method will simulate that!
:::

## How It Works

This method combines two or more states in order to re-use and read them in permutations or conditions. For example, a block with the English alphabet letters will need 26 values. You can use less values by using combinations.

## The Logic

What your code will do going of by the example above is the following:

```
1 & 1 = A   1 & 5 = E   1 & 9 =  I   1 & 13 = M
1 & 2 = B   1 & 6 = F   1 & 10 = J
1 & 3 = C   1 & 7 = G   1 & 11 = K
1 & 4 = D   1 & 8 = H   1 & 12 = L
```

And then;

```
2 & 1 = N   2 & 5 = R   2 & 9 =  V   2 & 13 = Z
2 & 2 = O   2 & 6 = S   2 & 10 = W
2 & 3 = P   2 & 7 = T   2 & 11 = X
2 & 4 = Q   2 & 8 = U   2 & 12 = Y
```

Using this method, you achieve the same results with just 15 values. The more values available for combinations, the higher your state limit is.

:::tip
You can use more than 2 values in order to have more possible combinations.
:::

## How It Looks

Using the example above as reference, your states would look like this:

<CodeHeader>minecraft:block > description</CodeHeader>

```json
"states": {
    "wiki:value": [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13],
    "wiki:division": [1, 2]
}
```

And for your conditions, like this:

<CodeHeader>Permutation Condition</CodeHeader>

```molang
q.block_state('wiki:division') == 1 && q.block_state('wiki:value') == 1
```

<CodeHeader>Permutation Condition</CodeHeader>

```molang
q.block_state('wiki:division') == 1 && q.block_state('wiki:value') == 2
```
