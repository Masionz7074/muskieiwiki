---
title: Button Meta Hooks
category: Extensions
---

## Getting Started

Go to customizer and create a new script

## Examples

<CodeHeader>
    Basic Custom Meta Field Example
</CodeHeader>

```js
hook("customizer_meta:#TEST", ({buttons})=>{
  buttons.push({
    text: "Test Meta!!!!",
    icon: "textures/items/iron_sword",
    action(player) {
      player.success("Shitt worked!")
    }
  })
})
```

<CodeHeader>
    Resolving Icon IDs
</CodeHeader>

```js
icons.resolve("vanilla/iron_sword")
```

<CodeHeader>
    Advanced Player List Meta Field
</CodeHeader>

```js
// register meta field
hook("customizer_meta:#ADV_PLAYER_LIST", ({unprocessedButtonText, buttons, args, button, currView, getIcon, player, playerIsAllowed, meta, parseArgs})=>{
  for(const player2 of mc.world.getPlayers()) {
    // get config
    let json = {};
    try {
      json = JSON.parse(meta.split(' ').slice(1).join(' '))
    } catch {}

    // conditions
    if(json.excludeSelf && player.id == player2.id) continue;
    if(json.requiredTag2 && !player2.hasTag(json.requiredTag2)) continue;
    if(button.requiredTag && !playerIsAllowed(player, formatStr(button.requiredTag, player, {}, {player2: player2}))) continue;

    // add button
    buttons.push({
      text: parseArgs(formatStr(unprocessedButtonText, player, {}, {player2}), ...args),
      icon: icons.resolve(getIcon(button.iconID, button.iconOverrides, player2)),
      currView, // for view separator support
      action: (player)=>{
        // security
        if(button.disabled) return;

        // run actions
        for(const action of button.actions) {
          let result = actionParser.runAction(player, parseArgs(formatStr(action, player, {}, {player2}), ...args))
          if(!result && button.conditionalActions) break;
        }
      }
    })
  }
});
```