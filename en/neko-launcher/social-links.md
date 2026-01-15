# Social Links Configuration

The `socials` field allows instance authors to define official links for community, development, stores, and other resources. Neko Launcher automatically displays these with appropriate icons and colors.

---

## Overview

Social links appear in the instance details and provide quick access to:

* Community platforms (Discord, Telegram)
* Development repositories (GitHub, GitLab)
* Store / Donation pages
* Official websites
* Embedded video content

---

## Schema Structure

```typescript
socials: {
  url: string;
  type: string;
  label?: string;
}[];
```

### Fields

| Field             | Type         | Required | Description                                 |
| ----------------- | ------------ | -------- | ------------------------------------------- |
| `socials[].url`   | string (URI) | ✓        | Target URL                                  |
| `socials[].type`  | string       | ✓        | Platform key used to determine icon & color |
| `socials[].label` | string       |          | Custom label (for `website` and `iframe`)   |

* `socials` is **optional** in instance configuration
* Launcher automatically maps icon + color from `type`
* Unknown types use default styling
* `label` is only supported for `website` and `iframe` types

---

## Basic Example

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

## Supported Platforms

### Store & Commerce

| Platform     | Type Keys                    | Color |
| ------------ | ---------------------------- | ----- |
| Store / Shop | `store`, `shop`, `market`    | Green |

**Example:**
```json
{ "type": "store", "url": "https://shop.furi.moe" }
```

---

### Development Platforms

| Platform | Type Keys      | Color  |
| -------- | -------------- | ------ |
| GitHub   | `github`, `gh` | Purple |
| GitLab   | `gitlab`       | Orange |

**Example:**
```json
{ "type": "github", "url": "https://github.com/aomkoyo" }
```

---

### Social Media

| Platform   | Type Keys           | Color    |
| ---------- | ------------------- | -------- |
| Facebook   | `facebook`, `fb`    | Blue     |
| YouTube    | `youtube`, `yt`     | Red      |
| X (Twitter)| `x`, `twitter`      | Sky Blue |
| Instagram  | `instagram`, `ig`   | Pink     |
| TikTok     | `tiktok`            | Red/Pink |

**Example:**
```json
{ "type": "youtube", "url": "https://youtube.com/@channel" }
```

---

### Gaming & Streaming

| Platform | Type Keys       | Color   |
| -------- | --------------- | ------- |
| Discord  | `discord`, `dc` | Blurple |
| Twitch   | `twitch`        | Purple  |

**Example:**
```json
{ "type": "discord", "url": "https://discord.gg/alicemagic" }
```

---

### Chat Platforms

| Platform | Type Keys         | Color    |
| -------- | ----------------- | -------- |
| Telegram | `telegram`, `tg`  | Sky Blue |

**Example:**
```json
{ "type": "telegram", "url": "https://t.me/alicemagic" }
```

---

### Support & Donations

| Platform | Type Keys                    | Color |
| -------- | ---------------------------- | ----- |
| Donation | `patreon`, `kofi`, `support` | Coral |

**Example:**
```json
{ "type": "support", "url": "https://furipay.me/aomkoyo" }
```

---

### General & Media

| Platform      | Type Keys        | Color          | Custom Label |
| ------------- | ---------------- | -------------- | ------------ |
| Website       | `web`, `website` | Emerald        | ✓            |
| Video Embed   | `iframe`         | Violet (Modal) | ✓            |

**Example:**
```json
{ "type": "website", "url": "https://furi.moe" }
{ "type": "website", "url": "https://wiki.example.com", "label": "Wiki" }
{ "type": "iframe", "url": "https://www.youtube.com/embed/dQw4w9WgXcQ" }
{ "type": "iframe", "url": "https://www.youtube.com/embed/dQw4w9WgXcQ", "label": "Trailer" }
```

**Note:** 
* The `iframe` type is specifically for **video embeds** or **direct video links** only (YouTube embeds, Vimeo, etc.)
* The `label` field allows you to customize the display text for `website` and `iframe` links
* If label is not provided, the launcher will show a default label based on the type

---

### Default Fallback

Any unrecognized type will use default violet styling:

```json
{ "type": "custom", "url": "https://example.com" }
```

---

## Complete Platform Reference

| Category | Platform     | Key / Type                               | Color          |
| -------- | ------------ | ---------------------------------------- | -------------- |
| Store    | Store / Shop | `store`, `shop`, `market`                | Green          |
| Dev      | GitHub       | `github`, `gh`                           | Purple         |
|          | GitLab       | `gitlab`                                 | Orange         |
| Social   | Facebook     | `facebook`, `fb`                         | Blue           |
|          | YouTube      | `youtube`, `yt`                          | Red            |
|          | X (Twitter)  | `x`, `twitter`                           | Sky Blue       |
|          | Instagram    | `instagram`, `ig`                        | Pink           |
|          | TikTok       | `tiktok`                                 | Red/Pink       |
| Gaming   | Discord      | `discord`, `dc`                          | Blurple        |
|          | Twitch       | `twitch`                                 | Purple         |
| Chat     | Telegram     | `telegram`, `tg`                         | Sky Blue       |
| Support  | Donation     | `patreon`, `kofi`, `support`             | Coral          |
| General  | Website      | `web`, `website`                         | Emerald        |
| Media    | Video Embed  | `iframe`                                 | Violet (Modal) |
| Default  | Other        | *any*                                    | Violet         |

---

## Advanced Example

```json
{
  "$schema": "https://cdn.furimoe.com/schema/neko-launcher.json",
  "name": "alice-magic",
  "displayName": "Alice Magic",
  
  "socials": [
    {
      "type": "discord",
      "url": "https://discord.gg/alicemagic"
    },
    {
      "type": "github",
      "url": "https://github.com/alice-magic"
    },
    {
      "type": "store",
      "url": "https://shop.furi.moe"
    },
    {
      "type": "youtube",
      "url": "https://youtube.com/@alicemagic"
    },
    {
      "type": "support",
      "url": "https://furipay.me/aomkoyo"
    },
    {
      "type": "website",
      "url": "https://furi.moe"
    },
    {
      "type": "website",
      "url": "https://launcher.furi.moe/wiki",
      "label": "Wiki"
    },
    {
      "type": "iframe",
      "url": "https://www.youtube.com/embed/dQw4w9WgXcQ",
      "label": "Trailer"
    },
    {
      "type": "telegram",
      "url": "https://t.me/alicemagic"
    }
  ]
}
```

---

## Best Practices

### Order & Priority
* Place most important links first (usually Discord/community)
* Group related platforms together
* Limit to 3-7 links for better UX

### URL Validation
* Use HTTPS for all URLs
* Verify links are publicly accessible
* Test links before deployment
* Use official/canonical URLs

### Platform Selection
* Choose platforms your community actively uses
* Don't add links just for completeness
* Update links when platforms change

---

## See Also

* [Instance Configuration](instance-configuration.md) - Main instance configuration
* [Back to Documentation Index](README.md)
