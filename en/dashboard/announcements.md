# Announcements

Each instance has an announcement feed the launcher shows as a banner on the instance page: maintenance notices, events, release notes. Manage it in the **Announcements** tab of the instance.

---

## Writing an announcement

| Field | Notes |
|---|---|
| **Title** | The headline. Add a Thai (or other language) variant in the metadata, `th_title`. |
| **Category** | `NOTICE`, `NEWS` or `EVENT`; each has its own colour. |
| **Link** | Opened when the player clicks the banner. |
| **Date** | Shown on the banner; the feed is sorted newest first. |
| **Active** | Only active announcements are shown. Prepare one in advance and switch it on later. |
| **Image** | Optional banner image; a localized variant is `th_imageUrl`. |

Turn the banner on or off for the whole instance with **Announcements enabled** in the instance settings.

## How the launcher gets it

The instance carries an `announcementUrl` (`https://api.neko-launcher.com/api/v1/instances/<name>/announcements`). The launcher fetches it on the instance page, with the player's identity headers, and the API returns only active announcements the player may see (a locked instance answers an empty list to players without access).

Self-hosted instances can point `announcementUrl` at their own JSON file; the format is documented in [Announcement feed](../neko-launcher/announcement-instance.md).
