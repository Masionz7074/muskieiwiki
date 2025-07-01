---
category: Advanced
title: How to Use invites
tags:
    - advanced
---

<template-Stub />

## How do invites work?

In customizer, you can add a creation (called UI in pre-v3.0) of the "invite" type. This is like the base for invites

## How to create an invite

1. Go to `Config UI`
2. Go to `Customizer`
3. Click `Add`
4. Scroll down to "Invite"
5. Set the unique ID (remember, no spaces in invite unique ids! and they wont be displayed to the player)
6. Fill in the expiration time (if needed), remember: 20 ticks = 1 second, 1200 ticks = 1 minute

## Setting up the invites

When editing invites you can edit the actions of 4 things:
- Deny actions: ran when invite is denied
- Accept actions: ran when invite is accepted
- Expiration actions: ran when invite expires
- Send actions: ran when invite is sent

---

> You do need to be familiar with the formatting system, and something else to note is the formatting system has 2 copies of the player-related variables in some places. Like, `<name>` has a `<name2>` counterpart. This is for multiple players.
> Player 1 = The player who received the invite
> Player 2 = The player who sent the invite

## Creating the invite UI

Invites dont do anything on their own, so heres how you set up a UI

### Create the UI

This should just be a normal creating UI process, so you do have to be familiar with creating UIs.

### Create a send button

The action should be `/scriptevent leafgui:invite UNIQUE_ID`

:::tip
You can put a command in quotes after the leafgui:invite scriptevent, like:

/scriptevent leafgui:invite UNIQUE_ID "/scriptevent leaf:open your-invite-ui"

This will allow the invite UI to reopen your UI once the player is done interacting with it.
:::

### Create a incoming invites button

Create a button, and choose whatever icon (if wanted). The display should be something like `Invite from <name2>` (just an example, just gives you an idea)

Use whatever for the action, i recommend just putting a `/`

Now, the special part is after you create the button:
> Edit the button
> Edit meta to "#INVITES UNIQUE_ID" (`#INVITES` is case sensitive, make sure its all uppercase)

### Create a outgoing invites button

Repeat the steps from the incoming invites button (example text for this one would be: `Invite to <name>`)

This time, the meta should be "#INVITES UNIQUE_ID out" (`out` should be all lowercase)

## Test your UI

Open your UI and make sure it works. You might need another player (e.g. friend, alt account) online to help test it with you.