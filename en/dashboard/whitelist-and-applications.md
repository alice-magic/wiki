# Whitelist, Applications and Invite Links

How players get access to a locked instance: you add them, they apply, or they follow an invite link. All three end in the same place, a **whitelist entry**.

---

## Whitelist

**Whitelist** tab. Add a player by **Minecraft UUID** (with or without dashes) or by **username** (case-insensitive). UUID entries survive name changes; username entries are convenient for players who have not launched yet. Remove an entry to revoke access; the player's next launch fails the access check.

Whitelist entries count toward the workspace plan's limit. Applications approved through the form create entries here too, so one list is the source of truth for your Minecraft server plugin as well; see [Server API](../neko-launcher/server-api.md).

The whitelist only matters when the instance is locked: **PRIVATE**, or PUBLIC/UNLISTED with **Enforce whitelist** on. See [Instances](instances.md).

## The no-access message

Players who open a locked instance see a message at the bottom of the launcher instead of Play. Write it in **Settings → No-access message**, per language (EN, TH, JP, RU) with a title and a subtitle; the launcher picks the player's language and falls back to the default. Typical content: how to reach you, or that applications open on a certain date. When applications are open the launcher appends **Tap here to apply** automatically.

## Applications

**Applications** tab, gear icon → **Form settings**.

### Application mode

| Mode | Form | Result of applying |
|---|---|---|
| **CLOSED** (default) | – | No apply button. |
| **OPEN** | none | Whitelisted immediately. |
| **AUTO** | required | Answers validated, then whitelisted immediately. |
| **MANUAL** | required | Goes to the review queue; you approve or reject. |

Applications require a locked instance (PRIVATE or Enforce whitelist). OFFICIAL instances cannot take applications.

### Limits

- **Slot cap** — maximum whitelist size reachable through applications; `-1` for none. Checked at submit and again at approve.
- **Cool-down** — days a rejected player must wait before applying again; `-1` makes a rejection permanent.
- **Minimum age** — compared with the answer of the question you mark as the age question; below it the application is refused on submit.
- **Discord webhook** — posts every submission, decision and payment to a channel. The URL is stored as a secret and never shown again.

### Listing

Where the recruitment is advertised. It never changes who can install.

| Listing | Shown as *recruiting* in Discover |
|---|---|
| **LINK** (default) | No; players need the apply link or invite link. |
| **DISCOVERY** | After [discovery approval](discovery.md). |
| **OPEN** | As soon as applications are open, no approval. |

### Form builder

Questions come in these types: short text, long text, number, dropdown, radio, checkbox, date. Each has a label and description per language, *required*, and limits (length, min/max). Mark one number question as the **age question** for the minimum-age gate. Two questions can be **auto-filled** with the applicant's Minecraft name and UUID.

**Sections** turn the form into pages. A radio or dropdown answer can **jump** to a specific page, so a *Builder* and a *Developer* answer different follow-up questions; pages the applicant is routed around are not required.

**Theme**: banner, background colour or image, accent colour, font and primary language. The apply page at `https://neko-launcher.com/apply/<name>` and the launcher's apply modal both use it. Owners can walk their own form as a test; such a test application goes through the queue like any other.

### Reviewing

**Applications** tab → **Responses**. Filter by status, read the answers (folded per application), add a note, **Approve** or **Reject**. Approving writes the whitelist entry (or, with an [entry fee](entry-fee.md), asks the player to pay first). Export everything as CSV.

Applicants are notified in the launcher, on the website and by email when they applied on the website.

## Invite links

**Invites** tab. An invite link is `https://neko-launcher.com/j/<code>`:

- opened in a browser it shows the instance and offers **Open in Neko Launcher** and, when recruiting, **Apply**;
- inside the launcher (`nekolauncher://join/<code>`) it adds the instance and opens it.

Links can **expire** after a number of days and be limited to a **number of uses**; revoke one at any time. Codes are random by default. A **custom code** (a vanity word) is allowed once the instance has discovery approval, so known servers cannot be impersonated.

An invite link does not grant access by itself: the player still needs to be whitelisted or to apply. It is the shortest way to bring someone to the apply form.

## See also

- [Apply to a server (player side)](../how-to/apply-to-a-server.md)
- [Entry fee](entry-fee.md)
- [Deep links](../neko-launcher/deep-links.md)
