# DNS-based Instance Discovery

Neko Launcher สามารถค้นหาและติดตั้ง instance จาก **DNS TXT record** ได้ แทนที่จะส่ง URL ของ config ให้ผู้เล่น คุณสามารถแนบรายละเอียดของ instance ไว้กับโดเมนที่คุณควบคุมได้ แล้วผู้เล่นก็ติดตั้งได้เพียงแค่พิมพ์โดเมนนั้น (หรือ IP) ลงใน launcher

นี่คือวิธีที่แนะนำสำหรับการแจกจ่ายมอดแพ็กสาธารณะหรือเซิร์ฟเวอร์แพ็ก คุณเปลี่ยน URL เบื้องหลังได้ทุกเมื่อ และผู้เล่นทุกคนจะได้รับอัปเดตในการเปิดครั้งถัดไปโดยที่คุณไม่ต้องแชร์อะไรใหม่อีก

---

## ทำไมต้องใช้

- 🔎 **ตรวจจับอัตโนมัติ** — ผู้เล่นค้นหา instance ของคุณด้วยโดเมน ไม่ต้องคัดลอกและวาง URL ของ config
- 🔗 **การผสานรวมเซิร์ฟเวอร์** — ผูกที่อยู่เซิร์ฟเวอร์ Minecraft เข้ากับ launcher instance ของมันในที่เดียว
- ♻️ **อัปเดตอย่างไร้กังวล** — สลับ URL ของ config/manifest ที่อยู่เบื้องหลัง record ได้เลย ลิงก์ที่คุณแชร์ไปแล้วจะไม่มีวันล้าสมัย

---

## การค้นหาทำงานอย่างไร

เมื่อผู้เล่นป้อนโดเมน launcher จะ query DNS หา TXT record สองชื่อ ตามลำดับดังนี้:

1. `_nekolauncher.<domain>` — **หลัก (primary)**
2. `_alicemagiclauncher.<domain>` — **สำรอง (fallback)** (จะ query ก็ต่อเมื่อไม่พบ record หลักเท่านั้น)

record แรกที่แยกวิเคราะห์ได้สำเร็จจะเป็นผู้ชนะ นี่คือขั้นตอนทั้งหมดตั้งแต่การป้อนข้อมูลจนถึงการติดตั้ง:

```mermaid
sequenceDiagram
    actor Player
    participant Launcher
    participant DNS
    participant CDN as Config / Manifest Host

    Player->>Launcher: Enter domain or IP
    Launcher->>DNS: TXT _nekolauncher.<domain>
    alt primary missing
        Launcher->>DNS: TXT _alicemagiclauncher.<domain>
    end
    DNS-->>Launcher: v=2 record (key=value;...)
    Launcher->>Launcher: Parse v2 → instanceUrl, manifestUrl
    Launcher->>CDN: GET instanceUrl (X-UUID, online headers)
    CDN-->>Launcher: instance.json
    Launcher->>CDN: GET manifestUrl (X-UUID, online headers)
    CDN-->>Launcher: manifest.json (SHA-1 file list)
    Launcher-->>Player: Show instance details
    Player->>Launcher: Confirm install
    Launcher->>CDN: Download files, verify SHA-1
```

> launcher จะส่ง `X-UUID` (Minecraft UUID ของผู้เล่นในรูปแบบมีขีดคั่น) และ `online` (`"true"` สำหรับบัญชี Xbox/Microsoft จริง หรือ `"false"` ในกรณีอื่น) ไปพร้อมกับคำขอ config, manifest และไฟล์ ผู้ดูแลเซิร์ฟเวอร์สามารถใช้ค่าเหล่านี้ควบคุมการเข้าถึงได้ — ดู [HTTP Headers](http-headers.md)

---

## รูปแบบ TXT record (v2)

ค่าแบบ v2 คือ **รายการของคู่ `key=value` ที่คั่นด้วยเครื่องหมายอัฒภาค (semicolon)** โดยการจับคู่ key จะไม่คำนึงถึงตัวพิมพ์เล็ก-ใหญ่

```text
_nekolauncher.<subdomain>  TXT  "v=2;ip=<server>;instanceUrl=<config_url>;manifestUrl=<manifest_url>;update=<timestamp>"
```

### Keys

| Key            | จำเป็น | คำอธิบาย                                                        | ตัวอย่าง                                   |
| -------------- | :------: | ------------------------------------------------------------------ | ----------------------------------------- |
| `v`            |    ✅     | เวอร์ชันของรูปแบบ — ต้องเป็น `2`                                   | `2`                                       |
| `ip`           |    ✅     | ที่อยู่เซิร์ฟเวอร์ Minecraft                                         | `play.furi.moe`                           |
| `instanceUrl`  |    ✅     | URL ของไฟล์ config instance แบบ JSON (`instance.json`)             | `https://files.catbox.moe/9y5o9r.json`    |
| `manifestUrl`  |    ✅     | URL ของไฟล์ manifest แบบ JSON (`manifest.json`)                    | `https://files.catbox.moe/esias3.json`    |
| `update`       |    ➖     | Unix timestamp เป็น **มิลลิวินาที** — เพิ่มค่าเพื่อส่งสัญญาณว่ามีอัปเดต | `1768293879377`                           |
| `name`         |    ➖     | ชื่อที่แสดงก่อนที่ config จะโหลด                                     | `Alice Magic`                             |
| `version`      |    ➖     | ป้ายกำกับเวอร์ชันของ instance                                       | `1.4.0`                                   |
| `minecraftVersion` | ➖  | คำใบ้เวอร์ชัน Minecraft                                             | `1.21.8`                                  |
| `loaderType`   |    ➖     | `fabric` / `forge` / `quilt` / `neoforge`                          | `fabric`                                  |
| `loaderBuild`  |    ➖     | build/เวอร์ชันของ loader                                            | `0.17.2`                                  |
| `iconUrl`      |    ➖     | URL ของไอคอน instance                                              | `https://cdn.example.com/icon.png`        |
| `backgroundUrl`|    ➖     | URL ของภาพพื้นหลัง                                                 | `https://cdn.example.com/bg.png`          |
| `discordUrl`   |    ➖     | ลิงก์เชิญ Discord                                                  | `https://discord.gg/…`                    |
| `readonly`     |    ➖     | `true`/`false` — ล็อก instance ไม่ให้แก้ไขในเครื่อง                 | `false`                                   |
| `hideIp`       |    ➖     | `true`/`false` — ซ่อน IP ของเซิร์ฟเวอร์ใน UI                        | `false`                                   |

> **นามแฝงของ key (Key aliases):** `settings` ใช้เป็นนามแฝงของ `instanceUrl` ได้ และ `manifest` เป็นนามแฝงของ `manifestUrl` record เก่าที่ใช้ `settings=`/`manifest=` ยังคงใช้งานได้ — แต่ `instanceUrl`/`manifestUrl` คือชื่อมาตรฐาน ดังนั้นควรใช้ชื่อเหล่านี้กับ record ใหม่

### รูปแบบ pipe แบบเดิม (Legacy)

รูปแบบเก่าที่ **คั่นด้วย pipe** ยังคงถูกแยกวิเคราะห์ได้เพื่อความเข้ากันได้ย้อนหลัง:

```text
ip|instanceUrl|manifestUrl|iconUrl|backgroundUrl|discordUrl|version|name|loaderType|loaderBuild|readonly|hideIp|minecraftVersion
```

สำหรับสิ่งใหม่ ๆ ให้ใช้รูปแบบ key/value แบบ `v=2` — เพราะอ่านง่าย ไม่ขึ้นกับลำดับ และให้คุณละฟิลด์ที่ไม่จำเป็นได้

---

## ตัวอย่างการตั้งค่า

### Root domain

**โดเมน:** `furi.moe` · **เซิร์ฟเวอร์:** `play.furi.moe`

```text
Name:  _nekolauncher
Type:  TXT
Value: "v=2;ip=play.furi.moe;instanceUrl=https://files.catbox.moe/9y5o9r.json;manifestUrl=https://files.catbox.moe/esias3.json;update=1768293879377"
```

ผู้เล่นค้นหาได้โดยป้อน `furi.moe`

### Subdomain

**โดเมน:** `minecraft.example.com` · **เซิร์ฟเวอร์:** `mc.example.com`

```text
Name:  _nekolauncher.minecraft
Type:  TXT
Value: "v=2;ip=mc.example.com;instanceUrl=https://cdn.example.com/mc/instance.json;manifestUrl=https://cdn.example.com/mc/manifest.json;update=1768293879377"
```

ผู้เล่นค้นหาได้โดยป้อน `minecraft.example.com`

---

## ไฟล์ที่อยู่เบื้องหลัง URL

`instanceUrl` และ `manifestUrl` ต้องชี้ไปยัง JSON ที่ถูกต้องและเข้าถึงได้แบบสาธารณะผ่าน **HTTPS**

### Config ของ instance (`instanceUrl`)

`instance.json` แบบขั้นต่ำ ดูทุกฟิลด์ได้ที่ [Instance Configuration](instance-configuration.md)

```json
{
  "$schema": "https://cdn.neko-launcher.com/schema/neko-launcher.json",
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

### Manifest ของ instance (`manifestUrl`)

**อาร์เรย์** JSON ของไฟล์ แต่ละรายการต้องมี `path`, `url`, `size` และ `hash` แบบ **SHA-1** ดู [Instance Manifest](instance-manifest.md)

```json
[
  {
    "path": "mods/example-mod.jar",
    "url": "https://cdn.example.com/mods/example.jar",
    "size": 1234567,
    "hash": "2ef7bde608ce5404e97d5f042f95f89f1c232871"
  }
]
```

---

## การตั้งค่าผู้ให้บริการ DNS

ขั้นตอนเหมือนกันทุกที่ — สร้าง `TXT` record ที่มีชื่อว่า `_nekolauncher` (หรือ `_nekolauncher.<subdomain>`) โดยมีเนื้อหาเป็นสตริง v2 ที่อยู่ในเครื่องหมายคำพูด

| ผู้ให้บริการ      | ที่ไหน                    | ชื่อ record ที่ต้องป้อน                          |
| ----------------- | ------------------------ | --------------------------------------------- |
| **Cloudflare**    | DNS → Add record → `TXT` | `_nekolauncher` หรือ `_nekolauncher.<subdomain>`|
| **Route 53 (AWS)**| Hosted zone → Create record → `TXT` | `_nekolauncher`                    |
| **Google Cloud DNS** | Zone → Add record set → `TXT` | `_nekolauncher`                       |

**เนื้อหา / ค่า** (เหมือนกันทั้งหมด):

```text
"v=2;ip=play.furi.moe;instanceUrl=https://files.catbox.moe/9y5o9r.json;manifestUrl=https://files.catbox.moe/esias3.json;update=1768293879377"
```

---

## การทดสอบและตรวจสอบ

### อ่าน TXT record

```bash
# Linux / macOS
dig +short _nekolauncher.furi.moe TXT
```

```text
:: Windows
nslookup -type=TXT _nekolauncher.furi.moe
```

ผลลัพธ์ที่คาดหวัง:

```text
_nekolauncher.furi.moe. 300 IN TXT "v=2;ip=play.furi.moe;instanceUrl=https://files.catbox.moe/9y5o9r.json;manifestUrl=https://files.catbox.moe/esias3.json;update=1768293879377"
```

### ยืนยันว่า URL เข้าถึงได้

```bash
curl -I https://files.catbox.moe/9y5o9r.json
curl -I https://files.catbox.moe/esias3.json
```

ทั้งสองควรตอบกลับ `200 OK` พร้อม content type แบบ JSON

---

## แนวทางปฏิบัติที่ดี

**DNS**
- ใช้ TTL ต่ำ (300–600s) ระหว่างตั้งค่าเพื่อให้การเปลี่ยนแปลงแพร่กระจายเร็ว แล้วค่อยเพิ่มขึ้น (3600s+) เมื่อเสถียรแล้ว
- ให้บริการทุก URL ผ่าน **HTTPS** — ไม่รองรับ HTTP
- เพิ่มค่า `update` (timestamp เป็น ms) ทุกครั้งที่ config หรือ manifest เปลี่ยน เพื่อให้ไคลเอนต์รู้ว่าต้องรีเฟรช

**การจัดการ URL**
- วาง config และ manifest ไว้เบื้องหลัง CDN เพื่อความน่าเชื่อถือและความเร็ว
- เก็บทั้งสองไฟล์ไว้ในโดเมนเดียวกันเมื่อทำได้
- ส่ง CORS และ content-type header ที่ถูกต้อง

**ความปลอดภัย**
- HTTPS เท่านั้น
- ควบคุมการเข้าถึงด้วย [การตรวจสอบ HTTP header](http-headers.md) โดยใช้ `X-UUID` / `online`
- จำกัดอัตราการเรียก (rate-limit) และเฝ้าติดตาม endpoint ของ config/manifest

---

## การแก้ไขปัญหา

**ไม่พบ record**
- ยืนยันว่าชื่อคือ `_nekolauncher` เป๊ะ ๆ (หรือ `_nekolauncher.<subdomain>`) รวมถึงเครื่องหมายขีดล่างนำหน้าด้วย
- อย่าลืมว่า launcher จะลอง `_alicemagiclauncher.<domain>` ด้วย — ใช้ชื่อใดชื่อหนึ่งก็ได้
- เผื่อเวลาให้ DNS แพร่กระจาย และทดสอบกับ resolver หลายตัว

**รูปแบบไม่ถูกต้อง**
- ใช้คู่ `key=value` คั่นด้วย `;` โดยไม่มีช่องว่างรอบเครื่องหมายอัฒภาค
- ห่อค่าทั้งหมดด้วยเครื่องหมายคำพูด
- ใส่ key ที่จำเป็น: `v`, `ip` และ `instanceUrl` + `manifestUrl` (หรือนามแฝง `settings`/`manifest` ของมัน)

**URL โหลดไม่ได้**
- ยืนยันว่าใช้ HTTPS และเปิดไฟล์ในเบราว์เซอร์ได้
- ตรวจสอบ CORS และ content-type header
- ยืนยันว่า `$schema` ของ config และโครงสร้าง manifest ถูกต้อง (ดูหน้าอ้างอิงที่ลิงก์ไว้)

---

## ดูเพิ่มเติม

- [Instance Configuration](instance-configuration.md) — schema ของ `instance.json`
- [Instance Manifest](instance-manifest.md) — รายการไฟล์ `manifest.json` และการทำ SHA-1 hashing
- [HTTP Headers](http-headers.md) — การควบคุมการเข้าถึงด้วย `X-UUID` / `online`
- [Announcements](announcement-instance.md) — การเผยแพร่ประกาศภายใน launcher
- [Social Links](social-links.md) — การแนบลิงก์ชุมชนเข้ากับ instance
- [กลับไปยังสารบัญเอกสาร](README.md)
