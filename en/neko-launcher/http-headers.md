# HTTP Header Verification

Neko Launcher includes authentication information in every HTTP request, allowing server operators to implement access control, analytics, and anti-leech protection.

---

## Overview

Every HTTP request from the launcher includes headers identifying the player and their authentication status. This enables:

* **Access Control** - Restrict downloads to authorized players
* **Analytics** - Track instance usage and player demographics
* **Anti-Leech** - Prevent unauthorized redistribution
* **Player Verification** - Confirm legitimate launcher usage

---

## Request Headers

### Header Reference

| Header   | Type    | Description                                       | Example                                  |
| -------- | ------- | ------------------------------------------------- | ---------------------------------------- |
| `x-uuid` | string  | Player's Minecraft UUID                           | `8518f0b2-d106-4c39-88d5-c7da11c91bbe`   |
| `online` | boolean | Whether player is authenticated with Mojang/Microsoft | `true` or `false`                    |

---

## Header Details

### `x-uuid`

* **Format:** UUID v4 (hyphenated)
* **Example:** `8518f0b2-d106-4c39-88d5-c7da11c91bbe`
* **Always Present:** Yes
* **Unique:** Per player account

The player's Minecraft UUID identifies them uniquely across sessions.

### `online`

* **Format:** Boolean string
* **Values:** `true` or `false`
* **Always Present:** Yes

Indicates whether the player is using a legitimate Minecraft account:
* `true` - Authenticated with Mojang/Microsoft
* `false` - Offline/cracked account

---

## Example Request

```http
GET /instance.json HTTP/1.1
Host: cdn.furi.moe
x-uuid: 8518f0b2-d106-4c39-88d5-c7da11c91bbe
online: true
User-Agent: NekoLauncher/1.0
```

---

## Implementation Examples

### Node.js (Express)

```javascript
app.get('/instance.json', (req, res) => {
  const playerUUID = req.headers['x-uuid'];
  const isOnline = req.headers['online'] === 'true';

  // Verify authentication
  if (!isOnline) {
    return res.status(403).json({ error: 'Online mode required' });
  }

  // Check whitelist
  if (!isPlayerAllowed(playerUUID)) {
    return res.status(403).json({ error: 'Not whitelisted' });
  }

  // Log access
  logPlayerAccess(playerUUID, 'instance.json');

  // Serve config
  res.json(instanceConfig);
});
```

---

### PHP

```php
<?php
$playerUUID = $_SERVER['HTTP_X_UUID'] ?? null;
$isOnline = ($_SERVER['HTTP_ONLINE'] ?? 'false') === 'true';

// Require online mode
if (!$isOnline) {
    http_response_code(403);
    die(json_encode(['error' => 'Online mode required']));
}

// Verify UUID format
if (!preg_match('/^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$/i', $playerUUID)) {
    http_response_code(400);
    die(json_encode(['error' => 'Invalid UUID']));
}

// Check whitelist
if (!isPlayerWhitelisted($playerUUID)) {
    http_response_code(403);
    die(json_encode(['error' => 'Not whitelisted']));
}

// Log and serve
logAccess($playerUUID);
header('Content-Type: application/json');
echo file_get_contents('instance.json');
```

---

### Python (Flask)

```python
from flask import Flask, request, jsonify
import re

app = Flask(__name__)

UUID_PATTERN = re.compile(r'^[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}$', re.I)

@app.route('/instance.json')
def instance_config():
    player_uuid = request.headers.get('x-uuid')
    is_online = request.headers.get('online') == 'true'
    
    # Validate UUID
    if not player_uuid or not UUID_PATTERN.match(player_uuid):
        return jsonify({'error': 'Invalid UUID'}), 400
    
    # Require online mode
    if not is_online:
        return jsonify({'error': 'Online mode required'}), 403
    
    # Check whitelist
    if not is_player_whitelisted(player_uuid):
        return jsonify({'error': 'Not whitelisted'}), 403
    
    # Log access
    log_player_access(player_uuid, 'instance.json')
    
    # Serve config
    return send_file('instance.json')
```

---

### Nginx (Reverse Proxy)

```nginx
server {
    listen 443 ssl;
    server_name cdn.furi.moe;

    location /instance.json {
        # Require x-uuid header
        if ($http_x_uuid = "") {
            return 403;
        }

        # Require online mode
        if ($http_online != "true") {
            return 403;
        }

        # Pass to backend with headers
        proxy_pass http://backend:3000;
        proxy_set_header X-UUID $http_x_uuid;
        proxy_set_header Online $http_online;
    }
}
```

---

## Use Cases

### Access Control

Restrict instance access to specific players:

```javascript
const WHITELIST = [
  '8518f0b2-d106-4c39-88d5-c7da11c91bbe',
  'a1b2c3d4-e5f6-7890-abcd-ef1234567890'
];

function isAllowed(uuid) {
  return WHITELIST.includes(uuid);
}
```

### Online Mode Enforcement

Require legitimate Minecraft accounts:

```javascript
if (req.headers['online'] !== 'true') {
  return res.status(403).json({
    error: 'This instance requires a legitimate Minecraft account'
  });
}
```

### Rate Limiting

Prevent abuse per player:

```javascript
const rateLimit = new Map();

function checkRateLimit(uuid) {
  const key = `downloads:${uuid}`;
  const count = rateLimit.get(key) || 0;
  
  if (count > 10) {
    return false; // Too many requests
  }
  
  rateLimit.set(key, count + 1);
  setTimeout(() => rateLimit.delete(key), 3600000); // Reset after 1 hour
  
  return true;
}
```

### Analytics

Track player engagement:

```javascript
function logDownload(uuid, file) {
  analytics.track({
    event: 'file_download',
    uuid: uuid,
    file: file,
    timestamp: new Date(),
    online: req.headers['online'] === 'true'
  });
}
```

### Anti-Leech Protection

Prevent hotlinking and unauthorized mirrors:

```javascript
function validateRequest(req) {
  const uuid = req.headers['x-uuid'];
  const online = req.headers['online'];
  
  // Require both headers
  if (!uuid || !online) {
    return false;
  }
  
  // Validate UUID format
  if (!/^[0-9a-f-]{36}$/i.test(uuid)) {
    return false;
  }
  
  return true;
}
```

---

## Security Considerations

### Header Spoofing

**Risk:** Clients could potentially spoof headers.

**Mitigation:**
* Implement additional verification (API tokens, signatures)
* Use HTTPS to prevent MITM attacks
* Combine with IP-based rate limiting
* Monitor for suspicious patterns

### UUID Privacy

**Risk:** Player UUIDs are semi-public information.

**Best Practices:**
* Don't expose UUIDs publicly
* Hash UUIDs before logging to external services
* Comply with privacy regulations (GDPR, etc.)
* Provide opt-out mechanisms

### Rate Limiting

**Implementation:**
* Limit requests per UUID per hour
* Implement exponential backoff
* Block suspicious patterns
* Monitor for abuse

---

## Best Practices

### Validation

1. **Always validate headers** - Check presence and format
2. **Sanitize input** - Prevent injection attacks
3. **Use regex patterns** - Validate UUID format
4. **Type checking** - Verify boolean values

### Logging

1. **Log access** - Track who downloads what
2. **Redact sensitive data** - Hash UUIDs in external logs
3. **Monitor patterns** - Detect abuse early
4. **Retention policies** - Follow data protection laws

### Response Codes

Use appropriate HTTP status codes:
* `200 OK` - Successful request
* `400 Bad Request` - Invalid headers/UUID
* `403 Forbidden` - Not authorized
* `429 Too Many Requests` - Rate limit exceeded
* `500 Internal Server Error` - Server issues

---

## Testing

### Manual Testing with curl

```bash
# Test with headers
curl -H "x-uuid: 8518f0b2-d106-4c39-88d5-c7da11c91bbe" \
     -H "online: true" \
     https://cdn.furi.moe/instance.json

# Test without headers (should fail)
curl https://cdn.furi.moe/instance.json

# Test offline mode (should fail if restricted)
curl -H "x-uuid: 8518f0b2-d106-4c39-88d5-c7da11c91bbe" \
     -H "online: false" \
     https://cdn.furi.moe/instance.json
```

---

## See Also

* [DNS Discovery](dns-discovery.md) - Auto-discovery configuration
* [Instance Configuration](instance-configuration.md) - Config schema
* [Instance Manifest](instance-manifest.md) - Manifest schema
* [Back to Documentation Index](README.md)
