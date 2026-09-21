# Dashboard: Workspaces, Plans and Members

The dashboard at [neko-launcher.com/dashboard](https://neko-launcher.com/dashboard) is where a server owner publishes instances and manages players. Everything in it belongs to a **workspace**.

---

## Workspaces

A workspace is a team plus a plan. Signing in for the first time creates a personal workspace on the FREE plan; you can create more or be invited to others, and switch between them from the top bar. Instances, members, invite links, applications and billing are all per workspace.

## Plans

Plans are subscriptions on the workspace, paid by card or PromptPay (Stripe), PayPal or cryptocurrency. Limits at the time of writing:

| Plan | Instances | Storage | Whitelist | Members | Discovery listing | Automatic slip verification |
|---|---|---|---|---|---|---|
| FREE | 1 | 20 MB | 5 | 1 | – | – |
| SMALL | 2 | 128 MB | 10 | 2 | ✓ | – |
| STARTER | 3 | 256 MB | 25 | 3 | ✓ | – |
| PRO | 10 | 512 MB | 100 | 10 | ✓ | ✓ |
| UNLIMITED | 20 | 4 GB | 1000 | 50 | ✓ | ✓ |

Current prices and details are on the **Plan** page of the dashboard. Storage counts files you upload; Modrinth-hosted mods do not count. The whitelist limit counts every whitelisted player across the workspace, whether added by hand or through applications.

When a plan lapses, features tied to it stop (discovery listing, automatic slip verification) and limits apply again; your data stays.

## Members and roles

**Members** tab. Invite by email; the invitee accepts from their own dashboard.

| Role | Can |
|---|---|
| **OWNER** | Everything, including billing and deleting the workspace. |
| **ADMIN** | Everything except billing and ownership. |
| **MEMBER** | Only what is granted, per permission node. |

Permission nodes cover instances (create, settings, files, versions, delete), whitelist, applications review, announcements, members and the API key. A member of the owning workspace can always install its instances from the launcher when signed in on the website with the same account.

## API key

**Settings → API key** issues one key per workspace for your Minecraft server plugin to read the whitelist. See [Server API](../neko-launcher/server-api.md). Treat it as a password; regenerate it if it leaks.

## Notifications

The bell in the top bar collects application events (submitted, approved, rejected, payment received). Owners may also set a **Discord webhook** per instance and applicants who applied on the website receive email.

## What is in the sidebar

| Section | See |
|---|---|
| Instances | [Instances](instances.md), [Files and versions](files-and-versions.md) |
| Whitelist, Applications, Invites | [Whitelist and applications](whitelist-and-applications.md) |
| Entry fee | [Entry fee](entry-fee.md) |
| Announcements | [Announcements](announcements.md) |
| Discovery | [Discovery](discovery.md) |
| Plan, Members, API key | this page |
