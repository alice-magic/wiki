# Instance Configuration Schema (English)

This schema is used to configure a **Minecraft instance in Neko Launcher**.

* **$comment**: Schema for Minecraft instance configuration
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

## Additional Fields

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
| `ignored`                 | string[]     | Files or folders ignored during sync/packaging.              |
| `readonly`                | boolean      | Whether the instance is read-only.                           |
| `gameArgs`                | string[]     | Extra JVM / game arguments.                                  |
| `socials`                 | array        | Social / community / store links for the instance.           |

---

## Minecraft Configuration Example

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

```json
"metadata": {
  "wallpaper": "https://cdn.example.com/wallpaper.webp",
  "th_displayName": "Thai name",
  "th_description": "Thai description",
  "customField": "Additional info"
}
```

Localized fields use the pattern `{locale}_{field}` such as `th_displayName`, `ja_description`.

---

## Using Tags

```json
"tags": [
  "Survival",
  "Modded",
  "RPG",
  "Community"
]
```

See the supported tags list below.

---

## Ignored Files/Folders

```json
"ignored": [
  "logs",
  "crash-reports",
  "screenshots",
  "*.log",
  "saves/*/playerdata"
]
```

Supports exact paths, glob patterns, and nested paths.

---

## Game Arguments

```json
"gameArgs": [
  "--quickPlayMultiplayer=play.furi.moe",
  "-Xmx4G",
  "-XX:+UseG1GC"
]
```

---

## Complete Example

See the full example in the English section below.

---

## See Also

* [Instance Manifest](instance-manifest.md) - Define downloadable files
* [Social Links](social-links.md) - Configure community links
* [Back to Documentation Index](README.md)
