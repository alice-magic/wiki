# Deep Links

Ways to open a specific server in the launcher from outside it: a web invite link, a `nekolauncher://` URL, or a desktop shortcut.

---

## Invite links: `neko-launcher.com/j/<code>`

Created in the dashboard (**Invites** tab). Opening one in a browser shows the instance's name, icon and description and offers:

- **Open in Neko Launcher** — hands `nekolauncher://join/<code>` to the installed launcher. If nothing opens within a couple of seconds the page offers the download.
- **Apply** — when the instance takes applications, a link to `https://neko-launcher.com/apply/<name>`.

Inside the launcher the code is resolved against the API (so a revoked or expired link stops working everywhere), the instance is added to the sidebar and its page opens. A player without access sees the owner's no-access message and, when recruiting, the apply prompt.

Codes are random by default; a **custom code** requires discovery approval for the instance.

## URL scheme: `nekolauncher://`

| URL | Effect |
|---|---|
| `nekolauncher://join/<code>` | Same as the invite link above. |
| `nekolauncher://instance/<name>` | Opens an instance the player already has (or can install) by its name. The website's *Open in launcher* buttons use this. |

The scheme is registered on install (Windows, macOS, Linux). Older schemes `aml://`, `alicemagiclauncher://` and `nekolauncherprotocol://` still open the launcher. Values are limited to `A–Z a–z 0–9 . _ -` and 64 characters.

If the launcher is not running, the link starts it and is handled once the window is ready; if it is running, the existing window is focused.

## Desktop shortcuts

Right-click an instance in the launcher → **Create shortcut** puts a shortcut on the desktop that starts the launcher with `--instance <name>`, going straight to that instance.

## Apply page: `neko-launcher.com/apply/<name>`

The web version of the apply form, for players who prefer the browser. It requires a Microsoft sign-in and produces the same application as the launcher.

## See also

- [Whitelist, applications and invite links](../dashboard/whitelist-and-applications.md)
- [Player guide](../players/README.md)
