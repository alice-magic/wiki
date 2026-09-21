# การตั้งค่าอินสแตนซ์ (schema v2)

การตั้งค่าอินสแตนซ์อธิบายม็อดแพ็กหนึ่งชุด: ตัวตน เวอร์ชัน Minecraft และโหลดเดอร์ การนำเสนอ และวิธีที่ตัวเปิดปฏิบัติกับไฟล์ Neko API ให้บริการสำหรับอินสแตนซ์บนแดชบอร์ด ส่วนเจ้าของที่โฮสต์เองเขียนเป็น `instance.json`

- Schema: `https://cdn.neko-launcher.com/schema/neko-launcher.json` (เวอร์ชัน 2, 2026-09-21)
- Schema ก่อนหน้า (ล้าสมัย ตัวเปิดยังรับ): `https://cdn.neko-launcher.com/schema/neko-launcher-v1.json`

ใส่ `"$schema"` ในไฟล์เพื่อให้ editor ตรวจสอบและเติมให้

---

## รูปร่าง

ไฟล์ที่โฮสต์เองเป็นได้ทั้ง object เปล่าด้านล่างหรือ envelope ของ API `{ "code": 200, "message": "OK", "data": { … } }` ทั้งสองแบบผ่าน schema และตัวเปิดรับได้

```json
{
  "$schema": "https://cdn.neko-launcher.com/schema/neko-launcher.json",
  "name": "my-server",
  "displayName": "My Server",
  "description": "Fabric survival กับเพื่อน",
  "icon": "https://cdn.example.com/icon.png",
  "onlineMode": true,
  "minecraft": {
    "version": "1.21.8",
    "loader": { "type": "fabric", "build": "0.17.3", "enable": true }
  },
  "metadata": {
    "wallpaper": "https://cdn.example.com/wallpaper.webp",
    "announcementUrl": "https://cdn.example.com/announcements.json",
    "announcementEnabled": true,
    "no_access_title": "Members only",
    "th_no_access_title": "เฉพาะสมาชิก"
  },
  "gameArgs": ["--quickPlayMultiplayer=play.example.com"],
  "socials": [
    { "type": "discord", "url": "https://discord.gg/example" },
    { "type": "web", "url": "https://example.com", "label": "เว็บไซต์" }
  ],
  "tags": ["survival"],
  "ignored": ["options.txt", "resourcepacks", "shaderpacks", "screenshots", "logs"],
  "readonly": true,
  "hideMods": false,
  "activeVersion": "1.0.0",
  "changelog": "รุ่นแรก"
}
```

## ฟิลด์

### บังคับ

| ฟิลด์ | ชนิด | หมายเหตุ |
|---|---|---|
| `name` | string | `^[a-z0-9][a-z0-9-_]*$` ตัวระบุ และเป็นชื่อโฟลเดอร์ติดตั้งบนเครื่องผู้เล่น ห้ามเปลี่ยนหลังเผยแพร่ |
| `displayName` | string | แสดงให้ผู้เล่น |
| `minecraft.version` | string | `1.21.8`, `26.2` หรือ `latest` |
| `minecraft.loader` | object | `type` (`fabric`, `forge`, `quilt`, `neoforge`), `build`, `enable` ถ้า `enable: false` เกมเปิดแบบ vanilla |

### การนำเสนอ

| ฟิลด์ | ชนิด | หมายเหตุ |
|---|---|---|
| `description` | string หรือ null | |
| `icon` | URL หรือ null | |
| `metadata.wallpaper` | URL | รูปพื้นหลัง (แนะนำ WebP) |
| `metadata.wallpaper_video`, `wallpaper_type_video`, `wallpaper_video_status` | | แดชบอร์ดตั้งให้สำหรับพื้นหลังวิดีโอ (HLS) |
| `socials` | array | ดู [ลิงก์โซเชียล](social-links.md) |
| `tags` | array ของ string | ตัวกรอง Discover |

### พฤติกรรม

| ฟิลด์ | ชนิด | ค่าเริ่มต้น | หมายเหตุ |
|---|---|---|---|
| `onlineMode` | boolean | `true` | บัญชีออฟไลน์เล่นไม่ได้ |
| `readonly` | boolean | `true` | ไฟล์ที่จัดการเหมือน manifest เสมอ ไฟล์เกินในโฟลเดอร์ที่จัดการถูกลบ |
| `ignored` | array ของ string | `[]` | พาธที่เขียนครั้งเดียว ไม่เขียนทับหรือลบ ใส่ `mods/.connector` สำหรับแพ็กที่ใช้ Sinytra Connector |
| `gameArgs` | array ของ string | `[]` | อาร์กิวเมนต์เกมเพิ่มเติม |
| `hideMods` | boolean | `false` | ม็อดถูกเก็บนอก `mods/` และฉีดตอนเปิดเกม เฉพาะอินสแตนซ์บนแดชบอร์ด |
| `metadata.announcementUrl` | URL | | ฟีดประกาศ JSON array เปล่า ดู [ฟีดประกาศ](announcement-instance.md) |
| `metadata.announcementEnabled` | boolean | | แสดงแบนเนอร์ |

### สิทธิ์ (อินสแตนซ์บนแดชบอร์ด เป็นข้อมูลประกอบสำหรับโฮสต์เอง)

| ฟิลด์ | หมายเหตุ |
|---|---|
| `visibility` | `OFFICIAL`, `PUBLIC`, `UNLISTED`, `PRIVATE` |
| `enforceWhitelist` | ล็อกอินสแตนซ์ PUBLIC/UNLISTED ไว้กับไวต์ลิสต์ |
| `access` | API คำนวณให้ผู้เล่นที่เรียก ไฟล์ที่โฮสต์เองไม่สนใจค่านี้ |
| `applicationMode` | `OPEN`, `AUTO`, `MANUAL`, `CLOSED` |
| `metadata.no_access_title`, `metadata.no_access_subtitle` | ข้อความสำหรับผู้เล่นที่ไม่มีสิทธิ์ |

### ข้อความแยกภาษาใน `metadata`

key ใดใน metadata นำหน้าด้วยรหัสภาษาสองตัวได้: `th_no_access_title`, `en_no_access_subtitle`, `jp_…`, `ru_…` ตัวเปิดเลือกภาษาของผู้เล่นและใช้ key ที่ไม่มีคำนำหน้าเมื่อไม่มี key ที่ไม่รู้จักถูกเก็บไว้

### เวอร์ชัน

| ฟิลด์ | หมายเหตุ |
|---|---|
| `activeVersion` | ข้อความเวอร์ชันของชุดไฟล์ปัจจุบัน |
| `changelog` | Markdown แสดงในประวัติเวอร์ชันของตัวเปิด |

## ต่างจาก schema v1

- `description` และ `icon` เป็น `null` ได้
- เพิ่ม: `visibility`, `enforceWhitelist`, `tags`, `hideMods`, `activeVersion`, `changelog`, `applicationMode`, `access`, key วอลเปเปอร์วิดีโอ, `announcementEnabled`, ข้อความ `no_access_*`
- `socials[].type` เพิ่ม `furipay` (FuriPay handle ใน `url`) และทุกรายการมี `label` ได้
- `metadata` รับ key เพิ่มเติมได้
- รับ envelope ของ API ที่ระดับบนสุด
- ไฟล์ v1 ยังเป็นเอกสาร v2 ที่ถูกต้อง

## ดูเพิ่มเติม

- [Manifest ของอินสแตนซ์](instance-manifest.md)
- [DNS discovery](dns-discovery.md)
- [อินสแตนซ์ในแดชบอร์ด](../dashboard/instances.md)
