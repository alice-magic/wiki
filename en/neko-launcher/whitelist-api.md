# Whitelist API and Webhooks

Manage an instance's whitelist from your own tools: add players with an expiry, change or clear expiries in bulk, remove players, and get a signed webhook call whenever the list changes.

There are three ways in:

| Way in | Authentication | Can do |
|---|---|---|
| **[Server API](server-api.md)** | An API key (`x-api-key`) from **Developer → API keys** | List, check, add, change expiry, remove; report online players. Best for plugins, bots and backends. |
| **Dashboard API** (this page) | Bearer token of a signed-in workspace member with the *instance settings* permission | Everything below |
| **MCP** (AI assistants) | OAuth, through **Settings → Connected apps** | The same actions as tools; see [MCP tools](#mcp-tools) |

Base URL: `https://api.neko-launcher.com/api/v1`. Responses use the envelope `{ code, message, data }`. Interactive documentation is at `https://api.neko-launcher.com/docs`.

---

## Routes

All routes take the instance name (its URL id) as `<name>`. The [Server API](server-api.md) has the same write routes under `/server/instances/<name>/…`.

### List entries

```
GET /instances/<name>/whitelist
```

`data` is every entry, including expired ones: `id`, `matchType` (`uuid` or `username`), `minecraftUuid`, `username`, `createdAt`, `expiresAt`.

### Add a player

```
POST /instances/<name>/whitelist
```

```json
{ "value": "069a79f4-44e9-4726-a5be-fca90e38aaf5", "expiresAt": "2026-12-31T23:59:00+07:00" }
```

| Field | Required | Meaning |
|---|---|---|
| `value` | yes | A UUID (with or without dashes) or a username. |
| `matchType` | no | `uuid` or `username`. Omit it and the API decides: a UUID matches by UUID, anything else by username. |
| `username` | no | Display name stored next to a UUID entry. |
| `expiresAt` | no | Future ISO 8601 date-time with a time zone. Access ends at that moment. Omit or `null` for no expiry. |

Answers `201` with the entry, `409` for a duplicate, `403` when the plan's whitelist limit is reached. Expired entries do not count toward the limit.

### Set or clear one expiry

```
PATCH /instances/<name>/whitelist/<id>
{ "expiresAt": "2026-12-31T23:59:00Z" }
```

`null` makes the entry permanent. Giving an expired entry a new expiry brings it back, so it counts toward the limit again.

### Set or clear many expiries

```
PATCH /instances/<name>/whitelist/bulk
{ "ids": ["<id>", "<id>"], "expiresAt": "2026-12-31T23:59:00Z" }
```

Up to **1000** ids per request; ids from other instances are ignored. `data` is `{ "updated": <count>, "expiresAt": … }`. When the change would bring back more expired entries than the limit has room for, nothing changes and the API answers `403`.

### Remove players

```
DELETE /instances/<name>/whitelist/<id>
POST   /instances/<name>/whitelist/bulk-remove   { "ids": ["<id>", …] }
```

Bulk remove takes up to 1000 ids and answers `{ "removed": <count>, "ids": [...] }`.

## Webhooks

Send whitelist and application changes to your own service, for example to sync a game server or post to a Discord bot.

Webhooks belong to the workspace. Add them in the dashboard under **Developer → Webhooks**:

1. Paste the endpoint URL and press **Test** — the dashboard sends a `ping` and shows the answer.
2. Pick the events, and optionally limit the webhook to some instances.
3. Save, and copy the **signing secret** (`whsec_…`). It is shown once; **Rotate secret** issues a new one.

A workspace can have up to 10 webhooks. The URL must be public HTTPS without credentials. The dashboard lists the last 50 deliveries of each webhook with their status.

> A webhook URL set on an instance before October 2026 (*Whitelist → Webhook on whitelist changes*) was moved automatically to a workspace webhook with the three whitelist events. It now also gets signed requests.

### Events

| Event | When |
|---|---|
| `whitelist.added` | A player is added by hand, by import, by the API, or by an approved application (including after an entry fee is paid). Not sent for duplicates. |
| `whitelist.updated` | An expiry is set or cleared, for one entry or in bulk. |
| `whitelist.removed` | An entry is removed, one or in bulk. |
| `application.submitted` | A player sends an application. |
| `application.approved` / `application.rejected` | An application is decided. |
| `application.payment_required` | An application is approved and an entry fee is requested. |
| `payment.received` / `payment.approved` / `payment.rejected` | An entry-fee slip is handed in and checked. |
| `ping` | The dashboard's **Test** button. |

Each event is an HTTPS `POST` with `content-type: application/json` and these headers:

| Header | Value |
|---|---|
| `x-neko-event` | The event name. |
| `x-neko-delivery` | A unique id for this delivery. |
| `x-neko-timestamp` | Unix time in seconds when it was sent. |
| `x-neko-signature` | `sha256=` + hex HMAC-SHA256 of `<timestamp>.<raw body>` with the webhook's secret. |

A bulk change sends one request per entry, one after another.

```json
{
  "event": "whitelist.updated",
  "deliveryId": "3f0c9a…",
  "occurredAt": "2026-10-02T10:00:00.000Z",
  "instance": { "name": "koyo-7f3a", "displayName": "KoyoSMP" },
  "player": { "matchType": "uuid", "username": "Notch", "uuid": "069a79f444e94726a5befca90e38aaf5" },
  "whitelist": { "createdAt": "2026-09-01T08:00:00.000Z", "expiresAt": "2026-12-31T16:59:00.000Z" }
}
```

Application and payment events carry `"application": { "id", "applicantUsername", "applicantUuid", "reviewNote"?, "fee"?, "deadline"? }` instead of `player` and `whitelist`.

### Verifying the signature

Compute the HMAC over the **raw** body exactly as received (before any JSON parsing), compare in constant time, and refuse timestamps older than a few minutes so a captured request cannot be replayed.

```js
import { createHmac, timingSafeEqual } from 'node:crypto';

function verify(rawBody, headers, secret) {
  const ts = headers['x-neko-timestamp'];
  if (Math.abs(Date.now() / 1000 - Number(ts)) > 300) return false;
  const expected = 'sha256=' + createHmac('sha256', secret).update(`${ts}.${rawBody}`).digest('hex');
  const got = String(headers['x-neko-signature'] ?? '');
  return got.length === expected.length && timingSafeEqual(Buffer.from(got), Buffer.from(expected));
}
```

The [SDK](server-api.md#sdk) does this for you with `constructEvent()`.

### Delivery

Delivery is best-effort: a request times out after 5 seconds and is not retried. Answer with any `2xx` quickly and do the work afterwards. If you must never miss a change, also read the list periodically with the [Server API](server-api.md).

```mermaid
sequenceDiagram
    participant D as Dashboard, API or MCP
    participant N as Neko API
    participant Y as Your service
    D->>N: add, update or remove entries
    N-->>D: result
    N->>Y: signed POST, one per entry
    Y-->>N: 2xx
```

## MCP tools

AI assistants connected through MCP (`https://api.neko-launcher.com/api/v1/mcp`) get the same actions:

| Tool | Does |
|---|---|
| `whitelist_list` | List entries. |
| `whitelist_add` | Add by `value`, optional `matchType` and `expiresAt`. |
| `whitelist_set_expiry` | Set or clear the expiry of up to 1000 entries (`ids`, `expiresAt`). |
| `whitelist_remove` | Remove one entry. |
| `whitelist_remove_many` | Remove up to 1000 entries; asks you to confirm first. |

They need the *players* permission on the connection and the workspace plan's web MCP.

## See also

- [Server API and SDK](server-api.md)
- [Whitelist, applications and invite links](../dashboard/whitelist-and-applications.md)
