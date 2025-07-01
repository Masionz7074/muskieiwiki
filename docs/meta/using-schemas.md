---
title: Using Schemas
mentions:
    - SirLich
    - MedicalJewel105
    - 7dev7urandom
    - KalmeMarq
description: Using schemas in add-on development for VSCode.
---

A JSON schema gives you two things: validation to be sure that your JSON has the correct structure and (depending on editor support) IntelliSense to help you write your JSON correctly, to begin with. Schemas are nice because they give you instant feedback when you screw something up, but they can't catch everything.

JSON schemas are just JSON files themselves and don't do anything on their own. You can write your own or use somebody else's. There's a handful of schemas for Bedrock out there already. Since none of the schemas are "official" (that I know of), and since Bedrock is a moving target, there will probably be some inaccuracies in any schema that you find. So keep that in mind: sometimes the issue will be in your code, sometimes the schema may be wrong. If you find a wrong schema, consider improving it and giving the author a pull request to our collective benefit.

To get the validation working, you'll need a validator. You have many options here, including editor-specific options.

## Schemas

Many schemas exist, with many minor differences. Try out different schemas and see which one works best for you:

| Author                                                                | Supports                                                                                                       | Note                                             |
|-----------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|--------------------------------------------------|
| [Assassin](https://github.com/aexer0e/bedrock-schema)                 | Behavior pack entity file                                                                                      | The original Schema this article was written for |
| [Tschrock's](https://github.com/bedrock-studio/bedrock-json-schemas/) | Manifest, Actor Animation Controller, Actor Animations, Actor Resource Definition, Render Controller, Geometry |                                                  |
| [stirante](https://github.com/stirante/bedrock-shader-schema/)        | Shaders                                                                                                        |                                                  |
| [KalmeMarq](https://github.com/KalmeMarq/Bugrock-JSON-UI-Schemas/)    | JSON UI files (including _ui_defs.json and _global_variables.json)                                             |                                                  |

## VSCode

To use this schema inside your JSON file in VSCode, simply add this line to your root object:

`"$schema": "https://aexer0e.github.io/bedrock-schema/"`

It should look like something like this:

<CodeHeader></CodeHeader>

```json
"format_version": "1.14.0",
"$schema": "https://aexer0e.github.io/bedrock-schema/"
```

### Adding Schema to Workspaces

If you want to utilize this schema to work with all of your files inside your Workspace, you can add it to your VS Code Workspace's settings.

To do this, make sure you're in your Workspace, then press `Ctrl+Shift+P` and type and select `>Preferences: Open Workspace Settings (JSON)`. After that, add this to the root object

<CodeHeader></CodeHeader>

```json
"settings": {
    "json.schemas": [
        {
            "fileMatch": [
                "*.json"
            ],
            "url": "https://aexer0e.github.io/bedrock-schema/"
        }
    ]
}
```

To test if it works, create a `.json` file, open an object, and see if you get the auto-completion options. (You can also press `Ctrl+Space` to force it into showing the available options.)
