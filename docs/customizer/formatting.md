---
title: Formatting
category: Basics
---

<template-Stub />

The formatting system can be used in anything, from sidebars, to UIs, and even nametags and chat customization

| Placeholder                   | Description                                                               |
|------------------------------|---------------------------------------------------------------------------|
| `<drj>`                      | Default rank joiner, useful for chat rank formats                         |
| `<msg>`                      | Player's message (only available in chat rank formats)                    |
| `<rc>`                       | Default rank color                                                        |
| `<nc>`                       | Name color                                                                |
| `<bc>`                       | Bracket color                                                             |
| `<mc>`                       | Message color                                                             |
| `<x>` `<y>` `<z>`            | Player's coordinates                                                      |
| `<name>`                     | Player's name                                                             |
| `<username>`                 | Alternative way to show player's name                                     |
| `<name_tag>`                 | Player's nametag (not available in nametags+)                             |
| `<xp>`                       | Player's XP                                                               |
| `<level>`                    | Player's XP level                                                         |
| `<hp>`                       | Player's HP                                                               |
| `<hp_max>`                   | Player's max HP (default 20)                                              |
| `<hp_min>`                   | Player's min HP (always 0)                                                |
| `<hp_default>`               | Player's default HP (should equal max)                                    |
| `<rank>`                     | Player's rank                                                             |
| `<kills>`                    | Player's kill count                                                       |
| `<deaths>`                   | Player's death count                                                      |
| `<blocks_broken>`            | Player's broken block count                                               |
| `<blocks_placed>`            | Player's placed block count                                               |
| `<cps>`                      | Player's CPS (clicks per second)                                          |
| `<clanID>`                   | Player's clan ID (not super useful)                                       |
| `<k/d>`                      | Player's kill/death ratio                                                 |
| `<claim>`                    | Land claim/zone the player is in                                          |
| `<tps>`                      | Server's TPS                                                              |
| `<online>`                   | Number of online players                                                  |
| `<day>`                      | In-game days                                                              |
| `<yr>`                       | Current year                                                              |
| `<mo>`                       | Current month (number form)                                               |
| `<mo/n>`                     | Current month (name form)                                                 |
| `<m>`                        | Current minute                                                            |
| `<h>`                        | Current hour (UTC)                                                        |
| `<s>`                        | Current seconds                                                           |
| `<ms>`                       | Current milliseconds                                                      |
| `<d>`                        | Current IRL day                                                           |
| `<dra>`                      | Double right arrow character                                              |
| `<lra>`                      | Double left arrow character                                               |
| `<moonPhase>`                | Current moon phase                                                        |
| `<h/12>`                     | Current hour (12-hour format)                                             |
| `<am/pm>`                    | AM or PM                                                                  |
| `<days_played>`              | Total days the player has played                                          |
| `<hours_played>`             | Total hours the player has played                                         |
| `<minutes_played>`           | Total minutes the player has played                                       |
| `<seconds_played>`           | Total seconds the player has played                                       |
| <code v-pre>{{score objective}}</code>      | Player's score for given objective |
| <code v-pre>{{scoreshort objective}}</code> | Player's score (shortened, e.g., 1.2k) |

## Examples

> <code v-pre>Hello, my name is &lt;name&gt; and i have ${{score money}}</code>
> 
> Example output: `Hello, my name is TrashyDaFox and i have $20`

## Rank Joiner

<code v-pre>{{rank_joiner "separator"}}</code>

seperator = the text between each rank (if multiple)

### Example

If the player has 3 ranks: Rank1, Rank2, Rank3 and the seperator is "; " the output will be:

Rank1; Rank2; Rank3

## Clans

```leafformat
{{clan "text if player is in clan" "text if player is not in clan (optional)"}}
```

### Examples

<CodeHeader>
    Show clan name and level
</CodeHeader>

```leafformat
{{clan "§a[@CLAN] §7§r| §r§aLvl. [@LVL]" "§7No Clan"}}
```

Output:

> FoxCult | Lvl. 0

## Has Tag

```leafformat
{{has_tag "text if player has tag" "text if player does not have tag"}}

OR:

{{has_tag "text if player has tag" "<bl>"}} - No text if player does not have tag
```