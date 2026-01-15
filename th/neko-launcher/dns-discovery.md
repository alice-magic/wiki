# DNS-based Instance Discovery

Neko Launcher รองรับการค้นหา instance อัตโนมัติโดยใช้ DNS TXT records ทำให้ผู้เล่นสามารถค้นหาและติดตั้ง instance ของคุณได้โดยเพียงแค่ใส่ชื่อโดเมนหรือที่อยู่ IP

---

## Overview

การค้นหาผ่าน DNS ช่วยให้:
* **ตรวจจับอัตโนมัติ** - ผู้เล่นค้นหา instance โดยไม่ต้องใส่ URL ด้วยตนเอง
* **การผสานรวมเซิร์ฟเวอร์** - เชื่อมโยงเซิร์ฟเวอร์ Minecraft กับ launcher instances
* **อัปเดตง่าย** - เปลี่ยน URL ของ instance โดยไม่ต้องแจกจ่ายลิงก์ใหม่

---

## TXT Record Format

สร้าง DNS TXT record ด้วยรูปแบบดังนี้:

```text
_nekolauncher.{subdomain} TXT "v=2;ip={server};settings={config_url};manifest={manifest_url};update={timestamp}"
```

### Record Components

| Component        | Description                               | ตัวอย่าง                                   |
| ---------------- | ----------------------------------------- | ------------------------------------------ |
| `_nekolauncher`  | Prefix คงที่สำหรับ launcher discovery     | `_nekolauncher`                            |
| `{subdomain}`    | Subdomain หรือ root (`@`)                 | `play`, `server`, `@`                      |
| `v`              | เวอร์ชันรูปแบบ record (ปัจจุบันคือ `2`)    | `2`                                        |
| `ip`             | ที่อยู่เซิร์ฟเวอร์ Minecraft               | `play.furi.moe`                            |
| `settings`       | URL ของไฟล์การตั้งค่า instance แบบ JSON   | `https://files.catbox.moe/9y5o9r.json`     |
| `manifest`       | URL ของไฟล์ manifest ของ instance แบบ JSON | `https://files.catbox.moe/esias3.json`     |
| `update`         | Unix timestamp เป็นมิลลิวินาที             | `1768293879377`                            |

### Semicolon-Separated Format

ค่า TXT record ใช้เครื่องหมายอัฒภาค `;` เป็นตัวคั่นด้วยคู่ key-value:
```text
"v=2;ip=server.address;settings=https://config.url;manifest=https://manifest.url;update=timestamp"
```

---

## Setup Examples

### ตัวอย่างที่ 1: Root Domain

**โดเมน:** `furi.moe`  
**เซิร์ฟเวอร์:** `play.furi.moe`

**DNS Record:**
```text
Name:  _nekolauncher
Type:  TXT
Value: "v=2;ip=play.furi.moe;settings=https://files.catbox.moe/9y5o9r.json;manifest=https://files.catbox.moe/esias3.json;update=1768293879377"
```

ผู้เล่นสามารถค้นหาได้โดยใส่: `furi.moe`

---

### ตัวอย่างที่ 2: Subdomain

**โดเมน:** `minecraft.example.com`  
**เซิร์ฟเวอร์:** `mc.example.com`

**DNS Record:**
```text
Name:  _nekolauncher.minecraft
Type:  TXT
Value: "v=2;ip=mc.example.com;settings=https://cdn.example.com/mc/config.json;manifest=https://cdn.example.com/mc/manifest.json;update=1768293879377"
```

ผู้เล่นสามารถค้นหาได้โดยใส่: `minecraft.example.com`

---

### ตัวอย่างที่ 3: Server Subdomain

**โดเมน:** `play.alicemagic.net`  
**เซิร์ฟเวอร์:** `play.alicemagic.net`

**DNS Record:**
```text
Name:  _nekolauncher.play
Type:  TXT
Value: "v=2;ip=play.alicemagic.net;settings=https://files.alicemagic.net/config.json;manifest=https://files.alicemagic.net/manifest.json;update=1768293879377"
```

ผู้เล่นสามารถค้นหาได้โดยใส่: `play.alicemagic.net`

---

## Discovery Process

1. **การป้อนของผู้เล่น** - ผู้ใช้ใส่โดเมน/IP ใน launcher
2. **DNS Query** - Launcher ทำ query `_nekolauncher.{domain}` TXT record
3. **แยกวิเคราะห์การตอบกลับ** - ดึงข้อมูลเซิร์ฟเวอร์, config URL และ manifest URL
4. **ดึงข้อมูล Configuration** - ดาวน์โหลดการตั้งค่า instance
5. **แสดง Instance** - แสดงรายละเอียด instance ให้ผู้เล่น
6. **ติดตั้ง** - ผู้เล่นยืนยันและ launcher ดาวน์โหลดไฟล์

---

## Configuration Files

ตรวจสอบให้แน่ใจว่า URL ของคุณชี้ไปยังไฟล์ JSON ที่ถูกต้อง:

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

1. ไปที่การจัดการ DNS
2. คลิก "Add record"
3. Type: `TXT`
4. Name: `_nekolauncher` (หรือ `_nekolauncher.subdomain`)
5. Content: `"v=2;ip=play.furi.moe;settings=https://files.catbox.moe/9y5o9r.json;manifest=https://files.catbox.moe/esias3.json;update=1768293879377"`
6. บันทึก

### Route 53 (AWS)

1. เลือก hosted zone
2. Create record
3. Record type: `TXT`
4. Record name: `_nekolauncher`
5. Value: `"v=2;ip=play.furi.moe;settings=https://files.catbox.moe/9y5o9r.json;manifest=https://files.catbox.moe/esias3.json;update=1768293879377"`
6. Create records

### Google Cloud DNS

1. ไปที่ Cloud DNS
2. เลือก zone
3. Add record set
4. Resource record type: `TXT`
5. DNS name: `_nekolauncher`
6. TXT data: `"v=2;ip=play.furi.moe;settings=https://files.catbox.moe/9y5o9r.json;manifest=https://files.catbox.moe/esias3.json;update=1768293879377"`
7. Create

---

## Testing & Validation

### ทดสอบ DNS Record

**ใช้ `dig` (Linux/macOS):**
```bash
dig _nekolauncher.furi.moe TXT
```

**ใช้ `nslookup` (Windows):**
```cmd
nslookup -type=TXT _nekolauncher.furi.moe
```

**ผลลัพธ์ที่คาดหวัง:**
```text
_nekolauncher.furi.moe. 300 IN TXT "v=2;ip=play.furi.moe;settings=https://files.catbox.moe/9y5o9r.json;manifest=https://files.catbox.moe/esias3.json;update=1768293879377"
```

### ตรวจสอบ URLs

ทดสอบว่า URL ทั้งสองสามารถเข้าถึงได้:
```bash
curl -I https://cdn.furi.moe/instance.json
curl -I https://cdn.furi.moe/manifest.json
```

ทั้งคู่ควรส่งกลับ `200 OK`

---

## Best Practices

### การตั้งค่า DNS
* ใช้ TTL ต่ำ (300-600s) ในระหว่างการตั้งค่าเริ่มต้นเพื่ออัปเดตอย่างรวดเร็ว
* เพิ่ม TTL (3600s+) เมื่อมีเสถียรภาพแล้ว
* ใช้ HTTPS สำหรับ URL ทั้งหมด
* ตรวจสอบว่า URL สามารถเข้าถึงได้สาธารณะ

### การจัดการ URL
* ใช้ CDN เพื่อประสิทธิภาพและความน่าเชื่อถือที่ดีขึ้น
* เก็บ config และ manifest ในโดเมนเดียวกันถ้าเป็นไปได้
* ใช้ URL แบบเวอร์ชันสำหรับการอัปเดตใหญ่
* ใช้ CORS headers ที่เหมาะสม

### ความปลอดภัย
* ใช้ HTTPS เท่านั้น (ไม่ใช่ HTTP)
* ใช้[การตรวจสอบ HTTP header](http-headers.md)
* พิจารณา rate limiting บน config/manifest endpoints
* ตรวจสอบรูปแบบการเข้าถึงที่ผิดปกติ

### การบำรุงรักษา
* ทดสอบการเปลี่ยนแปลง DNS ก่อนเปิดใช้งานจริง
* เก็บสำรองการตั้งค่าที่ใช้งานได้
* บันทึกการเปลี่ยนแปลงใน version control
* แจ้งผู้เล่นเกี่ยวกับการอัปเดตใหญ่

---

## Troubleshooting

### DNS Record Not Found
* ยืนยันว่าชื่อ record ถูกต้อง: `_nekolauncher`
* ตรวจสอบว่า subdomain ตรงกับการป้อนของผู้ใช้
* รอการแพร่กระจาย DNS (สูงสุด 48 ชั่วโมง)
* ทดสอบกับเซิร์ฟเวอร์ DNS หลายตัว

### รูปแบบไม่ถูกต้อง
* ตรวจสอบว่าตัวคั่นเครื่องหมายอัฒภาค `;` ถูกต้อง
* ใช้รูปแบบ key=value สำหรับฟิลด์ทั้งหมด
* ห่อค่าทั้งหมดด้วยเครื่องหมายคำพูด
* ไม่มีช่องว่างรอบเครื่องหมายอัฒภาค
* ฟิลด์ที่จำเป็นทั้งหมด: v, ip, settings, manifest, update

### URLs ไม่โหลด
* ยืนยันว่าใช้ HTTPS (ไม่ใช่ HTTP)
* ทดสอบ URL ในเบราว์เซอร์
* ตรวจสอบ CORS headers
* ตรวจสอบว่าไฟล์มี content-type ที่ถูกต้อง

---

## See Also

* [Instance Configuration](instance-configuration.md) - Schema ของไฟล์การตั้งค่า
* [Instance Manifest](instance-manifest.md) - Schema ของไฟล์ Manifest
* [HTTP Headers](http-headers.md) - Headers สำหรับการยืนยันตัวตน
* [กลับไปยังสารบัญเอกสาร](README.md)
