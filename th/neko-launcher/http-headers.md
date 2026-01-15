# HTTP Header Verification

Neko Launcher รวมข้อมูลการยืนยันตัวตนในทุก HTTP request ทำให้ผู้ดูแลเซิร์ฟเวอร์สามารถใช้งานการควบคุมการเข้าถึง, การวิเคราะห์ และการป้องกันการใช้ในทางที่ผิดได้

---

## Overview

ทุก HTTP request จาก launcher จะมี headers ที่ระบุตัวตนผู้เล่นและสถานะการยืนยันตัวตนของพวกเขา สิ่งนี้ช่วยให้สามารถ:

* **การควบคุมการเข้าถึง** - จำกัดการดาวน์โหลดให้กับผู้เล่นที่ได้รับอนุญาต
* **การวิเคราะห์** - ติดตามการใช้งาน instance และข้อมูลผู้เล่น
* **ป้องกันการใช้โดยไม่ได้รับอนุญาต** - ป้องกันการแจกจ่ายที่ไม่ได้รับอนุญาต
* **การยืนยันผู้เล่น** - ยืนยันการใช้งาน launcher ที่ถูกต้อง

---

## Request Headers

### Header Reference

| Header   | Type    | Description                                                 | ตัวอย่าง                                 |
| -------- | ------- | ----------------------------------------------------------- | ---------------------------------------- |
| `x-uuid` | string  | UUID ของผู้เล่น Minecraft                                   | `8518f0b2-d106-4c39-88d5-c7da11c91bbe`   |
| `online` | boolean | ว่าผู้เล่นได้รับการยืนยันตัวตนกับ Mojang/Microsoft หรือไม่ | `true` หรือ `false`                      |

---

## Header Details

### `x-uuid`

* **รูปแบบ:** UUID v4 (มีขีดกลาง)
* **ตัวอย่าง:** `8518f0b2-d106-4c39-88d5-c7da11c91bbe`
* **มีอยู่เสมอ:** ใช่
* **เฉพาะเจาะจง:** ต่อบัญชีผู้เล่น

UUID ของผู้เล่น Minecraft ระบุตัวตนพวกเขาอย่างเฉพาะเจาะจงข้ามเซสชัน

### `online`

* **รูปแบบ:** Boolean string
* **ค่า:** `true` หรือ `false`
* **มีอยู่เสมอ:** ใช่

แสดงว่าผู้เล่นใช้บัญชี Minecraft ที่ถูกต้องหรือไม่:
* `true` - ได้รับการยืนยันตัวตนกับ Mojang/Microsoft
* `false` - บัญชี Offline/cracked

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

### การควบคุมการเข้าถึง

จำกัดการเข้าถึง instance ให้กับผู้เล่นที่เฉพาะเจาะจง:

```javascript
const WHITELIST = [
  '8518f0b2-d106-4c39-88d5-c7da11c91bbe',
  'a1b2c3d4-e5f6-7890-abcd-ef1234567890'
];

function isAllowed(uuid) {
  return WHITELIST.includes(uuid);
}
```

### บังคับใช้ Online Mode

กำหนดให้ต้องมีบัญชี Minecraft ที่ถูกต้อง:

```javascript
if (req.headers['online'] !== 'true') {
  return res.status(403).json({
    error: 'This instance requires a legitimate Minecraft account'
  });
}
```

### Rate Limiting

ป้องกันการใช้ในทางที่ผิดต่อผู้เล่น:

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

### การวิเคราะห์

ติดตามการมีส่วนร่วมของผู้เล่น:

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

### การป้องกันการใช้โดยไม่ได้รับอนุญาต

ป้องกัน hotlinking และ mirrors ที่ไม่ได้รับอนุญาต:

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

### การปลอมแปลง Header

**ความเสี่ยง:** ไคลเอนต์อาจปลอมแปลง headers ได้

**การแก้ไข:**
* ใช้การยืนยันเพิ่มเติม (API tokens, signatures)
* ใช้ HTTPS เพื่อป้องกันการโจมตี MITM
* รวมกับ rate limiting แบบใช้ IP
* ตรวจสอบรูปแบบที่น่าสงสัย

### ความเป็นส่วนตัวของ UUID

**ความเสี่ยง:** UUID ของผู้เล่นเป็นข้อมูลกึ่งสาธารณะ

**แนวทางปฏิบัติที่ดีที่สุด:**
* อย่าเปิดเผย UUID สาธารณะ
* Hash UUID ก่อนบันทึกไปยังบริการภายนอก
* ปฏิบัติตามกฎระเบียบความเป็นส่วนตัว (GDPR เป็นต้น)
* ให้กลไกสำหรับการปฏิเสธ

### Rate Limiting

**การใช้งาน:**
* จำกัด requests ต่อ UUID ต่อชั่วโมง
* ใช้ exponential backoff
* บล็อกรูปแบบที่น่าสงสัย
* ตรวจสอบการใช้ในทางที่ผิด

---

## Best Practices

### การตรวจสอบความถูกต้อง

1. **ตรวจสอบ headers เสมอ** - ตรวจสอบการมีอยู่และรูปแบบ
2. **ทำความสะอาด input** - ป้องกันการโจมตี injection
3. **ใช้รูปแบบ regex** - ตรวจสอบรูปแบบ UUID
4. **ตรวจสอบประเภท** - ยืนยันค่า boolean

### การบันทึก

1. **บันทึกการเข้าถึง** - ติดตามว่าใครดาวน์โหลดอะไร
2. **ซ่อนข้อมูลที่ละเอียดอ่อน** - Hash UUID ในการบันทึกภายนอก
3. **ตรวจสอบรูปแบบ** - ตรวจจับการใช้ในทางที่ผิดตั้งแต่เนิ่นๆ
4. **นโยบายการเก็บรักษา** - ปฏิบัติตามกฎหมายคุ้มครองข้อมูล

### Response Codes

ใช้รหัสสถานะ HTTP ที่เหมาะสม:
* `200 OK` - Request สำเร็จ
* `400 Bad Request` - Headers/UUID ไม่ถูกต้อง
* `403 Forbidden` - ไม่ได้รับอนุญาต
* `429 Too Many Requests` - เกิน rate limit
* `500 Internal Server Error` - ปัญหาเซิร์ฟเวอร์

---

## Testing

### การทดสอบด้วยตนเองด้วย curl

```bash
# ทดสอบด้วย headers
curl -H "x-uuid: 8518f0b2-d106-4c39-88d5-c7da11c91bbe" \
     -H "online: true" \
     https://cdn.furi.moe/instance.json

# ทดสอบโดยไม่มี headers (ควรล้มเหลว)
curl https://cdn.furi.moe/instance.json

# ทดสอบ offline mode (ควรล้มเหลวถ้าถูกจำกัด)
curl -H "x-uuid: 8518f0b2-d106-4c39-88d5-c7da11c91bbe" \
     -H "online: false" \
     https://cdn.furi.moe/instance.json
```

---

## See Also

* [DNS Discovery](dns-discovery.md) - การตั้งค่าการค้นหาอัตโนมัติ
* [Instance Configuration](instance-configuration.md) - Schema การตั้งค่า
* [Instance Manifest](instance-manifest.md) - Schema Manifest
* [กลับไปยังสารบัญเอกสาร](README.md)
