# Social Links

`socials` puts buttons under the instance description: community, code, store, website, an embedded video, a donation handle. The launcher picks the icon and colour from `type`.

---

## Shape

```json
{
  "socials": [
    { "type": "discord", "url": "https://discord.gg/example" },
    { "type": "youtube", "url": "https://youtube.com/@example" },
    { "type": "iframe", "url": "https://www.youtube.com/watch?v=uTBy-PKrH3w" },
    { "type": "web", "url": "https://example.com", "label": "Website" },
    { "type": "furipay", "url": "aomkoyo" }
  ]
}
```

| Field | Required | Notes |
|---|---|---|
| `type` | yes | One of the keys below. Unknown types render with a neutral style. |
| `url` | yes | The link. For `furipay` it is the FuriPay handle, not a URL. |
| `label` | no | Custom text; honoured for `web`/`website` and `iframe`. |

The whole field is optional. Dashboard owners edit it in the instance settings.

## Types

| Group | Keys | Rendered as |
|---|---|---|
| Community | `discord`, `dc`, `telegram`, `tg` | Brand icon and colour |
| Code | `github`, `gh`, `gitlab` | Brand icon |
| Social media | `facebook`, `fb`, `youtube`, `yt`, `x`, `twitter`, `instagram`, `ig`, `tiktok`, `twitch` | Brand icon |
| Store | `store`, `shop`, `market` | Storefront icon |
| Support | `patreon`, `kofi`, `support` | Heart icon |
| Website | `web`, `website` | Globe icon, `label` shown |
| Embed | `iframe` | Opens the URL in a modal inside the launcher (YouTube links are embedded as a player) |
| Donations | `furipay` | Opens the FuriPay page for the handle in `url` |
| Other | `other` | Generic link icon |

## See also

- [Instance configuration](instance-configuration.md)
