---
title: Creating UIs
category: Basics
tags:
    - easy
nav_order: 1
---

<template-Stub />

:::warning NOTE
This article specifically goes over Action Forms
:::

## Creating UI

- Open `Config UI`, and head over to `Customizer` page
- Click "add"
- Click "action form" (feel free to try out the other ui types though!)

## Filling in the properties

### Title (required)

The title is the text at the top of the form

### Body (optional)

The body is the text under the title, but above the buttons

### Scriptevent (required)

The scriptevent is how you open the UI. It is like a unique identifier

:::info
To open your UI, do `/scriptevent leaf:open UI_SCRIPTEVENT`.

Example: `/scriptevent leaf:open server-ui`
:::

### After cancel action

If the user closes the UI without clicking a button, you can run a command.

### UI Layout

This is pretty much the layout of the UI. This shouldnt affect it *too* much, but keep in mind CherryUI does have some exclusive features

:::info
Guides on this website that depend on CherryUI will have CherryUI tag
:::

---

You should have your UI now! :3