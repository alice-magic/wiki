# Instance Configuration (schema v2)

The instance configuration describes one modpack: identity, Minecraft version and loader, presentation, and how the launcher treats the files. The Neko API serves it for dashboard instances; self-hosted owners write it by hand as `instance.json`.

- Schema: `https://cdn.neko-launcher.com/schema/neko-launcher.json` (version 2, 2026-09-21)
- Previous schema (out of date, still accepted by the launcher): `https://cdn.neko-launcher.com/schema/neko-launcher-v1.json`

Add `"$schema"` to the file for validation and completion in editors.

---

## Shape

A self-hosted file is either the bare object below or the API envelope `{ "code": 200, "message": "OK", "data": { … } }`; both validate against the schema and both are accepted by the launcher.

```json
{
  "$schema": "https://cdn.neko-launcher.com/schema/neko-launcher.json",
  "name": "my-server",
  "displayName": "My Server",
  "description": "Fabric survival with friends.",
  "icon": "https://cdn.example.com/icon.png",
  "onlineMode": true,
  "minecraft": {
    "version": "1.21.8",
    "loader": { "type": "fabric", "build": "0.17.3", "enable": true }
  },
  "metadata": {
    "wallpaper": "https://cdn.example.com/wallpaper.webp",
    "announcementUrl": "https://cdn.example.com/announcements.json",
    "announcementEnabled": true,
    "no_access_title": "Members only",
    "th_no_access_title": "เฉพาะสมาชิก"
  },
  "gameArgs": ["--quickPlayMultiplayer=play.example.com"],
  "socials": [
    { "type": "discord", "url": "https://discord.gg/example" },
    { "type": "web", "url": "https://example.com", "label": "Website" }
  ],
  "tags": ["survival"],
  "ignored": ["options.txt", "resourcepacks", "shaderpacks", "screenshots", "logs"],
  "readonly": true,
  "hideMods": false,
  "activeVersion": "1.0.0",
  "changelog": "First release."
}
```

## Fields

### Required

| Field | Type | Notes |
|---|---|---|
| `name` | string | `^[a-z0-9][a-z0-9-_]*$`. Identifier; also the install folder name on players' machines. Must not change once published. |
| `displayName` | string | Shown to players. |
| `minecraft.version` | string | `1.21.8`, `26.2` or `latest`. |
| `minecraft.loader` | object | `type` (`fabric`, `forge`, `quilt`, `neoforge`), `build`, `enable`. With `enable: false` the game launches vanilla. |

### Presentation

| Field | Type | Notes |
|---|---|---|
| `description` | string or null | |
| `icon` | URL or null | |
| `metadata.wallpaper` | URL | Background image (WebP recommended). |
| `metadata.wallpaper_video`, `wallpaper_type_video`, `wallpaper_video_status` | | Set by the dashboard for video backgrounds (HLS). |
| `socials` | array | See [Social links](social-links.md). |
| `tags` | array of string | Discover filters. |

### Behaviour

| Field | Type | Default | Notes |
|---|---|---|---|
| `onlineMode` | boolean | `true` | Offline accounts cannot play. |
| `readonly` | boolean | `true` | Managed files are kept identical to the manifest; extra files in managed folders are removed. |
| `ignored` | array of string | `[]` | Paths written once and never overwritten or deleted. Include `mods/.connector` for packs using Sinytra Connector. |
| `gameArgs` | array of string | `[]` | Extra game arguments. |
| `hideMods` | boolean | `false` | Mods are kept outside `mods/` and injected at launch. Dashboard instances only. |
| `metadata.announcementUrl` | URL | | Feed of announcements, a bare JSON array; see [Announcement feed](announcement-instance.md). |
| `metadata.announcementEnabled` | boolean | | Show the banner. |

### Access (dashboard instances, informational for self-hosted)

| Field | Notes |
|---|---|
| `visibility` | `OFFICIAL`, `PUBLIC`, `UNLISTED`, `PRIVATE`. |
| `enforceWhitelist` | Lock a PUBLIC/UNLISTED instance to the whitelist. |
| `access` | Computed by the API for the calling player. Ignored in a self-hosted file. |
| `applicationMode` | `OPEN`, `AUTO`, `MANUAL`, `CLOSED`. |
| `metadata.no_access_title`, `metadata.no_access_subtitle` | Message shown to players without access. |

### Localized text in `metadata`

Any metadata key may be prefixed with a two-letter language code: `th_no_access_title`, `en_no_access_subtitle`, `jp_…`, `ru_…`. The launcher picks the player's language and falls back to the unprefixed key. Unknown keys are preserved.

### Versioning

| Field | Notes |
|---|---|
| `activeVersion` | Version string of the current file set. |
| `changelog` | Markdown, shown in the launcher's version history. |

## Differences from schema v1

- `description` and `icon` may be `null`.
- New: `visibility`, `enforceWhitelist`, `tags`, `hideMods`, `activeVersion`, `changelog`, `applicationMode`, `access`, video wallpaper keys, `announcementEnabled`, `no_access_*` text.
- `socials[].type` gains `furipay` (a FuriPay handle in `url`) and every entry may carry `label`.
- `metadata` accepts additional keys.
- The API envelope is accepted at the top level.
- v1 files remain valid v2 documents.

## See also

- [Instance manifest](instance-manifest.md)
- [DNS discovery](dns-discovery.md)
- [Instances in the dashboard](../dashboard/instances.md)
