# Instance Configuration Schema

Schema for configuring an **Neko Launcher Minecraft instance**.

* **$comment**: Schema for Neko Launcher Minecraft instance configuration
* **title**: `Neko Launcher Instance Schema`
* **type**: `object`

---

## Required Fields

| Field         | Type    | Description                                                                            |
| ------------- | ------- | -------------------------------------------------------------------------------------- |
| `name`        | string  | Unique identifier for the instance (lowercase letters, numbers, hyphens, underscores). |
| `displayName` | string  | Display name of the instance.                                                          |
| `description` | string  | Short description of the instance.                                                     |
| `onlineMode`  | boolean | Whether the instance requires online authentication.                                   |
| `minecraft`   | object  | Minecraft version and loader configuration.                                            |

---

## Properties

| Field                     | Type         | Description                                                  |
| ------------------------- | ------------ | ------------------------------------------------------------ |
| `icon`                    | string (URI) | URL to the instance icon image.                              |
| `minecraft.version`       | string       | Minecraft version (e.g. `1.21.8`, `latest`).                 |
| `minecraft.loader.type`   | string       | Loader type (`fabric`, `forge`, `quilt`, `neoforge`).        |
| `minecraft.loader.build`  | string       | Loader build version.                                        |
| `minecraft.loader.enable` | boolean      | Enable or disable mod loader.                                |
| `metadata.wallpaper`      | string (URI) | Wallpaper image URL.                                         |
| `metadata.[localized]`    | string       | Localized fields such as `th_displayName`, `th_description`. |
| `tags`                    | string[]     | Tags categorizing the instance type/features.                |
| `ignored`                 | string[]     | Files or folders ignored during launch/packaging.            |
| `readonly`                | boolean      | Whether the instance is read-only.                           |
| `gameArgs`                | string[]     | Extra JVM / game arguments.                                  |
| `socials`                 | array        | Social / community / store links for the instance.           |

---

## Minecraft Version Configuration

The `minecraft` object defines the game version and mod loader settings:

```json
"minecraft": {
  "version": "1.21.8",
  "loader": {
    "type": "fabric",
    "build": "0.17.2",
    "enable": true
  }
}
```

### Version Field
* Use specific versions like `1.21.8`, `1.20.1`
* Or use `latest` for the latest release

### Loader Types
* `fabric` - Fabric Loader
* `forge` - Forge Mod Loader
* `quilt` - Quilt Loader
* `neoforge` - NeoForge Loader

---

## Metadata & Localization

The `metadata` object supports custom fields including localized content:

```json
"metadata": {
  "wallpaper": "https://cdn.example.com/wallpaper.webp",
  "th_displayName": "ชื่อภาษาไทย",
  "th_description": "คำอธิบายภาษาไทย",
  "customField": "any custom metadata"
}
```

Localized fields use the pattern `{locale}_{field}` where:
* `locale` is a language code (e.g., `th`, `ja`, `zh`)
* `field` is the property being localized (typically `displayName` or `description`)

---

## Tags

Use the `tags` array to categorize your instance by gameplay type, features, or style:

```json
"tags": [
  "Survival",
  "Modded",
  "RPG",
  "Community"
]
```

### Supported Tags

**Game Modes:**
* `Survival` - Classic survival gameplay
* `SMP` - Survival Multiplayer
* `Creative` - Creative mode focused
* `Hardcore` - Hardcore difficulty
* `Adventure` - Adventure mode gameplay
* `Spectator` - Spectator mode enabled

**Server Types:**
* `Skyblock` - Skyblock gameplay
* `Prison` - Prison server style
* `Factions` - Factions warfare
* `Earth` - Earth map servers
* `Anarchy` - Anarchy gameplay
* `Lifesteal` - Lifesteal mechanics
* `Pixelmon` - Pixelmon mod
* `Cobblemon` - Cobblemon mod

**Minigames:**
* `Minigame` - General minigames
* `BedWars` - BedWars minigame
* `SkyWars` - SkyWars minigame
* `Parkour` - Parkour challenges
* `PvP` - Player vs Player focused

**Gameplay Style:**
* `RPG` - Role-playing game elements
* `MMO` - Massively multiplayer online
* `Magic` - Magic/spells systems
* `Dungeons` - Dungeon exploration
* `Quests` - Quest systems
* `Lore` - Story/lore focused

**Technical:**
* `Modded` - Includes mods
* `Vanilla` - Vanilla Minecraft
* `Crossplay` - Bedrock/Java crossplay
* `Voice Chat` - Voice chat integration
* `Custom Items` - Custom items/resources
* `Tech` - Technology/automation mods
* `Redstone` - Redstone focused

**Community:**
* `Economy` - Economy systems
* `Roleplay` - Roleplay oriented
* `Community` - Community focused
* `Events` - Regular events
* `Building` - Building focused
* `Towny` - Towny plugin/mod

---

## Ignored Files

Use the `ignored` array to exclude files/folders from sync or packaging:

```json
"ignored": [
  "logs",
  "crash-reports",
  "screenshots",
  "*.log",
  "saves/*/playerdata"
]
```

Supports:
* Exact paths: `logs`, `screenshots`
* Glob patterns: `*.log`, `crash-reports/*.txt`
* Nested paths: `saves/world/region`

---

## Game Arguments

Add custom JVM or game arguments via the `gameArgs` array:

```json
"gameArgs": [
  "--quickPlayMultiplayer=play.furi.moe",
  "-Xmx4G",
  "-XX:+UseG1GC"
]
```

Common use cases:
* Auto-connect to servers: `--quickPlayMultiplayer=server.address`
* Memory allocation: `-Xmx4G`, `-Xms2G`
* JVM flags: `-XX:+UseG1GC`

---

## Complete Example

```json
{
  "$schema": "https://cdn.furimoe.com/schema/neko-launcher.json",
  "name": "alice-magic",
  "displayName": "Alice Magic: Furiora's World",
  "description": "A modded Minecraft experience with adventure.",
  "icon": "https://cdn.masuru.in.th/HamF2Hsoirx4K9DL.webp",
  "onlineMode": true,

  "minecraft": {
    "version": "1.21.8",
    "loader": {
      "type": "fabric",
      "build": "0.17.2",
      "enable": true
    }
  },

  "metadata": {
    "th_displayName": "อลิซ เมจิก: โลกของฟูริโอร่า",
    "th_description": "ประสบการณ์มอด Minecraft ที่เต็มไปด้วยการผจญภัย",
    "wallpaper": "https://cdn.masuru.in.th/2025-11-18_13.webp"
  },

  "socials": [
    { "type": "github", "url": "https://github.com/aomkoyo" },
    { "type": "store", "url": "https://shop.furi.moe" },
    { "type": "support", "url": "https://furipay.me/aomkoyo" }
  ],

  "ignored": [
    "logs",
    "crash-reports",
    "screenshots"
  ],
  "tags": [
    "Survival",
    "Modded",
    "RPG",
    "Magic",
    "Community"
  ],
  "readonly": false,
  "gameArgs": [
    "--quickPlayMultiplayer=play.furi.moe"
  ]
}
```

---

## See Also

* [Instance Manifest](instance-manifest.md) - Define downloadable files for your instance
* [Social Links](social-links.md) - Configure community and social links
* [Back to Documentation Index](README.md)
