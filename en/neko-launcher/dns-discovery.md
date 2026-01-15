# DNS-based Instance Discovery

Neko Launcher supports automatic instance discovery using DNS TXT records. This allows players to find and install your instance by simply entering a domain or IP address.

---

## Overview

DNS discovery enables:
* **Auto-detection** - Players discover instances without manual URL entry
* **Server integration** - Link Minecraft servers with launcher instances
* **Easy updates** - Change instance URLs without redistributing links

---

## TXT Record Format

Create a DNS TXT record with the following format:

```text
_nekolauncher.{subdomain} TXT "v=2;ip={server};settings={config_url};manifest={manifest_url};update={timestamp}"
```

### Record Components

| Component        | Description                               | Example                                    |
| ---------------- | ----------------------------------------- | ------------------------------------------ |
| `_nekolauncher`  | Fixed prefix for launcher discovery       | `_nekolauncher`                            |
| `{subdomain}`    | Subdomain or root (`@`)                   | `play`, `server`, `@`                      |
| `v`              | Record format version (currently `2`)     | `2`                                        |
| `ip`             | Minecraft server address                  | `play.furi.moe`                            |
| `settings`       | URL to instance configuration JSON        | `https://files.catbox.moe/9y5o9r.json`     |
| `manifest`       | URL to instance manifest JSON             | `https://files.catbox.moe/esias3.json`     |
| `update`         | Unix timestamp in milliseconds            | `1768293879377`                            |

### Semicolon-Separated Format

The TXT record value uses semicolon `;` as delimiter with key-value pairs:
```text
"v=2;ip=server.address;settings=https://config.url;manifest=https://manifest.url;update=timestamp"
```

---

## Setup Examples

### Example 1: Root Domain

**Domain:** `furi.moe`  
**Server:** `play.furi.moe`

**DNS Record:**
```text
Name:  _nekolauncher
Type:  TXT
Value: "v=2;ip=play.furi.moe;settings=https://files.catbox.moe/9y5o9r.json;manifest=https://files.catbox.moe/esias3.json;update=1768293879377"
```

Players can discover by entering: `furi.moe`

---

### Example 2: Subdomain

**Domain:** `minecraft.example.com`  
**Server:** `mc.example.com`

**DNS Record:**
```text
Name:  _nekolauncher.minecraft
Type:  TXT
Value: "v=2;ip=mc.example.com;settings=https://cdn.example.com/mc/config.json;manifest=https://cdn.example.com/mc/manifest.json;update=1768293879377"
```

Players can discover by entering: `minecraft.example.com`

---

### Example 3: Server Subdomain

**Domain:** `play.alicemagic.net`  
**Server:** `play.alicemagic.net`

**DNS Record:**
```text
Name:  _nekolauncher.play
Type:  TXT
Value: "v=2;ip=play.alicemagic.net;settings=https://files.alicemagic.net/config.json;manifest=https://files.alicemagic.net/manifest.json;update=1768293879377"
```

Players can discover by entering: `play.alicemagic.net`

---

## Discovery Process

1. **Player Input** - User enters domain/IP in launcher
2. **DNS Query** - Launcher queries `_nekolauncher.{domain}` TXT record
3. **Parse Response** - Extract server, config URL, and manifest URL
4. **Fetch Configuration** - Download instance configuration
5. **Display Instance** - Show instance details to player
6. **Install** - Player confirms and launcher downloads files

---

## Configuration Files

Ensure your URLs point to valid JSON files:

### Instance Configuration (`settings`)
```json
{
  "$schema": "https://cdn.furimoe.com/schema/neko-launcher.json",
  "name": "alice-magic",
  "displayName": "Alice Magic: Furiora's World",
  "description": "A modded Minecraft experience",
  "onlineMode": true,
  "minecraft": {
    "version": "1.21.8",
    "loader": {
      "type": "fabric",
      "build": "0.17.2",
      "enable": true
    }
  }
}
```

### Instance Manifest (`manifest_url`)
```json
[
  {
    "path": "mods/example-mod.jar",
    "url": "https://cdn.example.com/mods/example.jar",
    "size": 1234567,
    "hash": "abc123..."
  }
]
```

---

## DNS Provider Setup

### Cloudflare

1. Navigate to DNS management
2. Click "Add record"
3. Type: `TXT`
4. Name: `_nekolauncher` (or `_nekolauncher.subdomain`)
5. Content: `"v=2;ip=play.furi.moe;settings=https://files.catbox.moe/9y5o9r.json;manifest=https://files.catbox.moe/esias3.json;update=1768293879377"`
6. Save

### Route 53 (AWS)

1. Select hosted zone
2. Create record
3. Record type: `TXT`
4. Record name: `_nekolauncher`
5. Value: `"v=2;ip=play.furi.moe;settings=https://files.catbox.moe/9y5o9r.json;manifest=https://files.catbox.moe/esias3.json;update=1768293879377"`
6. Create records

### Google Cloud DNS

1. Go to Cloud DNS
2. Select zone
3. Add record set
4. Resource record type: `TXT`
5. DNS name: `_nekolauncher`
6. TXT data: `"v=2;ip=play.furi.moe;settings=https://files.catbox.moe/9y5o9r.json;manifest=https://files.catbox.moe/esias3.json;update=1768293879377"`
7. Create

---

## Testing & Validation

### Test DNS Record

**Using `dig` (Linux/macOS):**
```bash
dig _nekolauncher.furi.moe TXT
```

**Using `nslookup` (Windows):**
```cmd
nslookup -type=TXT _nekolauncher.furi.moe
```

**Expected Output:**
```text
_nekolauncher.furi.moe. 300 IN TXT "v=2;ip=play.furi.moe;settings=https://files.catbox.moe/9y5o9r.json;manifest=https://files.catbox.moe/esias3.json;update=1768293879377"
```

### Verify URLs

Test that both URLs are accessible:
```bash
curl -I https://cdn.furi.moe/instance.json
curl -I https://cdn.furi.moe/manifest.json
```

Both should return `200 OK`.

---

## Best Practices

### DNS Configuration
* Use low TTL (300-600s) during initial setup for quick updates
* Increase TTL (3600s+) once stable
* Use HTTPS for all URLs
* Ensure URLs are publicly accessible

### URL Management
* Use CDN for better performance and reliability
* Keep config and manifest on same domain if possible
* Use versioned URLs for major updates
* Implement proper CORS headers

### Security
* Use HTTPS exclusively (not HTTP)
* Implement [HTTP header verification](http-headers.md)
* Consider rate limiting on config/manifest endpoints
* Monitor for unusual access patterns

### Maintenance
* Test DNS changes before going live
* Keep backup of working configuration
* Document changes in version control
* Notify players of major updates

---

## Troubleshooting

### DNS Record Not Found
* Verify record name is correct: `_nekolauncher`
* Check subdomain matches user input
* Wait for DNS propagation (up to 48 hours)
* Test with multiple DNS servers

### Invalid Format
* Ensure semicolon `;` delimiters are correct
* Use key=value format for all fields
* Wrap entire value in quotes
* No spaces around semicolons
* All required fields: v, ip, settings, manifest, update

### URLs Not Loading
* Verify HTTPS is used (not HTTP)
* Test URLs in browser
* Check CORS headers
* Ensure files have correct content-type

---

## See Also

* [Instance Configuration](instance-configuration.md) - Configuration file schema
* [Instance Manifest](instance-manifest.md) - Manifest file schema
* [HTTP Headers](http-headers.md) - Authentication headers
* [Back to Documentation Index](README.md)
