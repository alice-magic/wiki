# Social Links Configuration

The optional `socials` field lets an instance author advertise official links — community servers, source repositories, stores, donation pages, websites, and even embedded videos. Neko Launcher renders each entry as a button with an icon and brand color derived automatically from its `type`.

---

## 🔎 Overview

Social links appear on the instance details screen and give players one-click access to:

* Community platforms (Discord, Telegram)
* Source repositories (GitHub, GitLab)
* Stores and donation pages
* Official websites
* Embedded video content (opened in a modal)

`socials` lives inside the instance config (`instance.json`) alongside fields like `name`, `displayName`, and `minecraft`. See [Instance Configuration](instance-configuration.md) for the full file.

---

## 🧩 Schema

```typescript
socials: {
  type: string;   // platform key — picks the icon + color
  url: string;    // target URL (https recommended)
  label?: string; // custom display text (website + iframe only)
}[];
```

### Fields

| Field             | Type         | Required | Description                                       |
| ----------------- | ------------ | :------: | ------------------------------------------------- |
| `socials[].type`  | string       | ✓        | Platform key that determines the icon and color   |
| `socials[].url`   | string (URI) | ✓        | Target URL                                        |
| `socials[].label` | string       |          | Custom label — honored only for `website` and `iframe` |

Notes:

* The whole `socials` field is **optional**.
* The launcher maps an icon and color from `type` automatically.
* Unknown `type` values still render, using the default fallback styling.
* `label` is respected only for `website` and `iframe`; other types use their built-in label.

---

## ⚡ Basic Example

```json
{
  "socials": [
    { "type": "discord", "url": "https://discord.gg/example" },
    { "type": "github", "url": "https://github.com/username/repo" },
    { "type": "store", "url": "https://shop.example.com" }
  ]
}
```

---

## 📚 Supported Platforms

Each row lists the accepted `type` keys (any one works) and the color the launcher applies.

### Community & Chat

| Platform | Type Keys        | Color    |
| -------- | ---------------- | -------- |
| Discord  | `discord`, `dc`  | Blurple  |
| Telegram | `telegram`, `tg` | Sky Blue |

```json
{ "type": "discord", "url": "https://discord.gg/alicemagic" }
```

### Development

| Platform | Type Keys      | Color  |
| -------- | -------------- | ------ |
| GitHub   | `github`, `gh` | Purple |
| GitLab   | `gitlab`       | Orange |

```json
{ "type": "github", "url": "https://github.com/alice-magic" }
```

### Social Media

| Platform    | Type Keys         | Color    |
| ----------- | ----------------- | -------- |
| Facebook    | `facebook`, `fb`  | Blue     |
| YouTube     | `youtube`, `yt`   | Red      |
| X (Twitter) | `x`, `twitter`    | Sky Blue |
| Instagram   | `instagram`, `ig` | Pink     |
| TikTok      | `tiktok`          | Red/Pink |

```json
{ "type": "youtube", "url": "https://youtube.com/@channel" }
```

### Streaming

| Platform | Type Keys | Color  |
| -------- | --------- | ------ |
| Twitch   | `twitch`  | Purple |

```json
{ "type": "twitch", "url": "https://twitch.tv/alicemagic" }
```

### Store & Support

| Platform     | Type Keys                    | Color |
| ------------ | ---------------------------- | ----- |
| Store / Shop | `store`, `shop`, `market`    | Green |
| Donation     | `patreon`, `kofi`, `support` | Coral |

```json
{ "type": "store", "url": "https://shop.furi.moe" }
{ "type": "support", "url": "https://furipay.me/aomkoyo" }
```

### Website & Video Embed

These two types support a custom `label`.

| Platform    | Type Keys        | Color          | Custom Label |
| ----------- | ---------------- | -------------- | :----------: |
| Website     | `web`, `website` | Emerald        | ✓            |
| Video Embed | `iframe`         | Violet (Modal) | ✓            |

```json
{ "type": "website", "url": "https://furi.moe" }
{ "type": "website", "url": "https://wiki.example.com", "label": "Wiki" }
{ "type": "iframe", "url": "https://www.youtube.com/embed/dQw4w9WgXcQ" }
{ "type": "iframe", "url": "https://www.youtube.com/embed/dQw4w9WgXcQ", "label": "Trailer" }
```

The `iframe` type is intended for **video embeds** (YouTube embed URLs, Vimeo, direct video links). It opens in a modal rather than the system browser. When `label` is omitted, the launcher shows a default label based on the `type`.

### Default Fallback

Any unrecognized `type` still renders — it falls back to violet styling with a generic icon:

```json
{ "type": "custom", "url": "https://example.com" }
```

---

## 🗂 Complete Platform Reference

| Category   | Platform     | Type Keys                    | Color          |
| ---------- | ------------ | ---------------------------- | -------------- |
| Community  | Discord      | `discord`, `dc`              | Blurple        |
|            | Telegram     | `telegram`, `tg`             | Sky Blue       |
| Dev        | GitHub       | `github`, `gh`               | Purple         |
|            | GitLab       | `gitlab`                     | Orange         |
| Social     | Facebook     | `facebook`, `fb`             | Blue           |
|            | YouTube      | `youtube`, `yt`              | Red            |
|            | X (Twitter)  | `x`, `twitter`               | Sky Blue       |
|            | Instagram    | `instagram`, `ig`            | Pink           |
|            | TikTok       | `tiktok`                     | Red/Pink       |
| Streaming  | Twitch       | `twitch`                     | Purple         |
| Store      | Store / Shop | `store`, `shop`, `market`    | Green          |
| Support    | Donation     | `patreon`, `kofi`, `support` | Coral          |
| General    | Website      | `web`, `website`             | Emerald        |
| Media      | Video Embed  | `iframe`                     | Violet (Modal) |
| Default    | Other        | *any*                        | Violet         |

---

## 🧪 Full Example

A `socials` array embedded in a real instance config:

```json
{
  "$schema": "https://cdn.neko-launcher.com/schema/neko-launcher.json",
  "name": "alice-magic",
  "displayName": "Alice Magic",
  "socials": [
    { "type": "discord", "url": "https://discord.gg/alicemagic" },
    { "type": "github", "url": "https://github.com/alice-magic" },
    { "type": "store", "url": "https://shop.furi.moe" },
    { "type": "youtube", "url": "https://youtube.com/@alicemagic" },
    { "type": "support", "url": "https://furipay.me/aomkoyo" },
    { "type": "website", "url": "https://furi.moe" },
    { "type": "website", "url": "https://neko-launcher.com/wiki", "label": "Wiki" },
    { "type": "iframe", "url": "https://www.youtube.com/embed/dQw4w9WgXcQ", "label": "Trailer" },
    { "type": "telegram", "url": "https://t.me/alicemagic" }
  ]
}
```

---

## ✅ Best Practices

**Order & priority**

* Put the most important link first — usually your Discord or main community hub.
* Group related platforms together (all social media, then dev, then store).
* Keep it to about 3–7 links so the row stays readable.

**URLs**

* Use `https://` everywhere.
* Point to official, canonical URLs and confirm they're publicly reachable.
* Test every link before shipping the instance.

**Selection**

* Add only platforms your community actually uses — skip links added purely for completeness.
* Keep them current; update or remove entries when a platform changes or dies.

---

## See Also

* [Instance Configuration](instance-configuration.md) — the full `instance.json` reference
* [Announcement Instance](announcement-instance.md) — advertise news and events in-launcher
* [Documentation Index](README.md)
