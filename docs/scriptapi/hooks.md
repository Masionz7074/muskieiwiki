---
title: Hooks
category: Basics
---

<CodeHeader>
Basic Hook Example
</CodeHeader>

```js
let ticks = 0;
let seconds = 0;
let tickHook = hook("tick", ()=>{
    ticks++;
    if(ticks % 20 == 0) {
        seconds++;
        mc.world.sendMessage("Hello, world!")
        if(seconds >= 5) {
            unhook(tickHook)
        }
    }
})
```