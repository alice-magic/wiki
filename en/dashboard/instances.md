# Instances and Their Settings

An instance is one modpack with its own files, version history, whitelist and settings. This page explains every setting and what it changes for players.

---

## Creating an instance

**Instances → New instance**: display name, description, Minecraft version, loader and build. The internal **name** (used in URLs and as the folder name on players' machines) is generated for you; instances created by API may supply a lowercase slug.

## General

| Setting | Effect |
|---|---|
| **Display name, description, icon** | Shown in the launcher and on the apply page. |
| **Wallpaper** | Background of the instance page in the launcher. Upload an image, or a video (MP4/WebM) that is converted to a streaming background. |
| **Tags** | Filters in Discover (`survival`, `roleplay`, …). |
| **Social links** | Buttons under the description: Discord, GitHub, YouTube, website, an embedded video, a FuriPay handle. See [Social links](../neko-launcher/social-links.md). |
| **Game arguments** | Extra arguments for the game, e.g. `--quickPlayMultiplayer=play.example.com` to connect on launch. |

## Who can see and who can install

Two settings work together. **Visibility** says who can see the instance; **Enforce whitelist** locks the files of a visible instance.

| Visibility | Listed in Discover | Who can install |
|---|---|---|
| **PRIVATE** (default) | Only to players who already have access, or when recruiting | Whitelisted players and workspace members |
| **UNLISTED** | No; reachable by name or invite link | Everyone who finds it, unless *Enforce whitelist* is on |
| **PUBLIC** | Yes, after [discovery approval](discovery.md) | Everyone, unless *Enforce whitelist* is on |
| **OFFICIAL** | Yes, on the launcher home screen | Everyone; whitelist ignored. Set by Neko admins only. |

The API computes an `access` flag for the calling player with this rule:

```
OFFICIAL                                   → allowed
PUBLIC or UNLISTED without enforceWhitelist → allowed
member of the owning workspace              → allowed
on the instance whitelist (UUID or name)    → allowed
otherwise                                   → denied
```

A player without access can still open the instance page in the launcher; **Play** is disabled and your **no-access message** is shown. See [Whitelist and applications](whitelist-and-applications.md) for the message and for applications.

## Online mode

On by default. Players on an offline (non-Microsoft) account see *This server requires a genuine Minecraft account* and cannot press Play. Turn it off only for servers that accept offline accounts.

## Read-only

On by default. The launcher keeps every managed file identical to the published version: changed files are restored and extra files in managed folders are removed on the next launch. Players keep control of paths in the **ignored** list. Turn it off for a pack players are expected to modify.

## Ignored paths

Paths (relative to the instance folder) that the launcher writes once and never touches again, and never deletes: player options, resource and shader packs, screenshots, logs, per-mod client configs. Presets cover common client mods (Sodium, Iris, Xaero, voice chat, …). Add `mods/.connector` when the pack uses Sinytra Connector: that folder is a per-machine cache and must not be shared.

## Hide mods

Off by default. When on, the launcher stores the instance's mods **outside** the `mods` folder on each player's machine and hands them to the mod loader at launch. Players can still add their own mods to `mods/`, but a mod that duplicates one of yours is refused. Supported on Fabric, Quilt, Forge and NeoForge, including packs that use Sinytra Connector. The launcher also strips its internal arguments from the game log so the hidden paths are not printed.

Use it to make a pack harder to copy wholesale; it is not encryption.

## Announcements

Each instance has an announcement feed the launcher shows in a banner. Manage it in the **Announcements** tab; see [Announcements](announcements.md).

## Danger zone

**Delete instance** removes files, versions, whitelist, applications and invite links. It cannot be undone.

## Setting summary

| Setting | Default | Changes for players |
|---|---|---|
| Visibility | PRIVATE | Whether they can find and install it |
| Enforce whitelist | off | Locks a PUBLIC/UNLISTED instance to the whitelist |
| Online mode | on | Offline accounts cannot play |
| Read-only | on | Managed files are kept identical to the published version |
| Ignored paths | preset | What the launcher never overwrites |
| Hide mods | off | Mods live outside `mods/` |
| Application mode | CLOSED | Whether players can apply |
| Entry fee | off | Whether approval requires payment |
