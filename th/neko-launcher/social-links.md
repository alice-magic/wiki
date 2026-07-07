# Social Links Configuration

ฟิลด์ `socials` (ตัวเลือก) ช่วยให้ผู้สร้าง instance สามารถโปรโมตลิงก์อย่างเป็นทางการได้ ไม่ว่าจะเป็นเซิร์ฟเวอร์ชุมชน, repository ซอร์สโค้ด, ร้านค้า, หน้าบริจาค, เว็บไซต์ หรือแม้แต่วิดีโอแบบฝังตัว Neko Launcher จะแสดงแต่ละรายการเป็นปุ่มพร้อมไอคอนและสีประจำแบรนด์ที่กำหนดขึ้นโดยอัตโนมัติจาก `type`

---

## 🔎 ภาพรวม

ลิงก์โซเชียลจะแสดงบนหน้ารายละเอียดของ instance และให้ผู้เล่นเข้าถึงสิ่งเหล่านี้ได้ในคลิกเดียว:

* แพลตฟอร์มชุมชน (Discord, Telegram)
* Repository ซอร์สโค้ด (GitHub, GitLab)
* ร้านค้าและหน้าบริจาค
* เว็บไซต์อย่างเป็นทางการ
* เนื้อหาวิดีโอแบบฝังตัว (เปิดใน modal)

`socials` อยู่ภายในไฟล์ตั้งค่าของ instance (`instance.json`) ควบคู่ไปกับฟิลด์อย่าง `name`, `displayName` และ `minecraft` ดูรายละเอียดไฟล์ทั้งหมดได้ที่ [Instance Configuration](instance-configuration.md)

---

## 🧩 Schema

```typescript
socials: {
  type: string;   // platform key — picks the icon + color
  url: string;    // target URL (https recommended)
  label?: string; // custom display text (website + iframe only)
}[];
```

### ฟิลด์

| ฟิลด์             | ชนิด         | จำเป็น | คำอธิบาย                                       |
| ----------------- | ------------ | :------: | ------------------------------------------------- |
| `socials[].type`  | string       | ✓        | คีย์แพลตฟอร์มที่กำหนดไอคอนและสี   |
| `socials[].url`   | string (URI) | ✓        | URL เป้าหมาย                                        |
| `socials[].label` | string       |          | ป้ายกำกับที่กำหนดเอง — มีผลเฉพาะ `website` และ `iframe` เท่านั้น |

หมายเหตุ:

* ฟิลด์ `socials` ทั้งหมดเป็น **ตัวเลือก**
* Launcher จะจับคู่ไอคอนและสีจาก `type` โดยอัตโนมัติ
* ค่า `type` ที่ไม่รู้จักก็ยังแสดงผลได้ โดยใช้การจัดรูปแบบสำรองเริ่มต้น
* `label` จะมีผลเฉพาะกับ `website` และ `iframe` เท่านั้น ประเภทอื่นจะใช้ป้ายกำกับที่มีมาให้ในตัว

---

## ⚡ ตัวอย่างพื้นฐาน

```json
{
  "socials": [
    { "type": "discord", "url": "https://discord.gg/example" },
    { "type": "github", "url": "https://github.com/username/repo" },
    { "type": "store", "url": "https://shop.example.com" }
  ]
}
```

---

## 📚 แพลตฟอร์มที่รองรับ

แต่ละแถวจะแสดงคีย์ `type` ที่ใช้ได้ (ใช้คีย์ใดก็ได้) และสีที่ launcher จะนำไปใช้

### ชุมชนและแชท

| Platform | Type Keys        | Color    |
| -------- | ---------------- | -------- |
| Discord  | `discord`, `dc`  | Blurple  |
| Telegram | `telegram`, `tg` | Sky Blue |

```json
{ "type": "discord", "url": "https://discord.gg/alicemagic" }
```

### การพัฒนา

| Platform | Type Keys      | Color  |
| -------- | -------------- | ------ |
| GitHub   | `github`, `gh` | Purple |
| GitLab   | `gitlab`       | Orange |

```json
{ "type": "github", "url": "https://github.com/alice-magic" }
```

### โซเชียลมีเดีย

| Platform    | Type Keys         | Color    |
| ----------- | ----------------- | -------- |
| Facebook    | `facebook`, `fb`  | Blue     |
| YouTube     | `youtube`, `yt`   | Red      |
| X (Twitter) | `x`, `twitter`    | Sky Blue |
| Instagram   | `instagram`, `ig` | Pink     |
| TikTok      | `tiktok`          | Red/Pink |

```json
{ "type": "youtube", "url": "https://youtube.com/@channel" }
```

### สตรีมมิ่ง

| Platform | Type Keys | Color  |
| -------- | --------- | ------ |
| Twitch   | `twitch`  | Purple |

```json
{ "type": "twitch", "url": "https://twitch.tv/alicemagic" }
```

### ร้านค้าและการสนับสนุน

| Platform     | Type Keys                    | Color |
| ------------ | ---------------------------- | ----- |
| Store / Shop | `store`, `shop`, `market`    | Green |
| Donation     | `patreon`, `kofi`, `support` | Coral |

```json
{ "type": "store", "url": "https://shop.furi.moe" }
{ "type": "support", "url": "https://furipay.me/aomkoyo" }
```

### เว็บไซต์และวิดีโอฝังตัว

สองประเภทนี้รองรับ `label` ที่กำหนดเองได้

| Platform    | Type Keys        | Color          | Custom Label |
| ----------- | ---------------- | -------------- | :----------: |
| Website     | `web`, `website` | Emerald        | ✓            |
| Video Embed | `iframe`         | Violet (Modal) | ✓            |

```json
{ "type": "website", "url": "https://furi.moe" }
{ "type": "website", "url": "https://wiki.example.com", "label": "Wiki" }
{ "type": "iframe", "url": "https://www.youtube.com/embed/dQw4w9WgXcQ" }
{ "type": "iframe", "url": "https://www.youtube.com/embed/dQw4w9WgXcQ", "label": "Trailer" }
```

ประเภท `iframe` มีไว้สำหรับ **วิดีโอฝังตัว** (YouTube embed URL, Vimeo, ลิงก์วิดีโอโดยตรง) โดยจะเปิดใน modal แทนที่จะเปิดในเบราว์เซอร์ของระบบ เมื่อไม่ได้ระบุ `label` launcher จะแสดงป้ายกำกับเริ่มต้นตาม `type`

### การจัดรูปแบบสำรองเริ่มต้น

ค่า `type` ใดก็ตามที่ไม่รู้จักก็ยังแสดงผลได้ โดยจะใช้การจัดรูปแบบสีม่วง (violet) พร้อมไอคอนทั่วไป:

```json
{ "type": "custom", "url": "https://example.com" }
```

---

## 🗂 อ้างอิงแพลตฟอร์มทั้งหมด

| Category   | Platform     | Type Keys                    | Color          |
| ---------- | ------------ | ---------------------------- | -------------- |
| Community  | Discord      | `discord`, `dc`              | Blurple        |
|            | Telegram     | `telegram`, `tg`             | Sky Blue       |
| Dev        | GitHub       | `github`, `gh`               | Purple         |
|            | GitLab       | `gitlab`                     | Orange         |
| Social     | Facebook     | `facebook`, `fb`             | Blue           |
|            | YouTube      | `youtube`, `yt`              | Red            |
|            | X (Twitter)  | `x`, `twitter`               | Sky Blue       |
|            | Instagram    | `instagram`, `ig`            | Pink           |
|            | TikTok       | `tiktok`                     | Red/Pink       |
| Streaming  | Twitch       | `twitch`                     | Purple         |
| Store      | Store / Shop | `store`, `shop`, `market`    | Green          |
| Support    | Donation     | `patreon`, `kofi`, `support` | Coral          |
| General    | Website      | `web`, `website`             | Emerald        |
| Media      | Video Embed  | `iframe`                     | Violet (Modal) |
| Default    | Other        | *any*                        | Violet         |

---

## 🧪 ตัวอย่างแบบเต็ม

อาร์เรย์ `socials` ที่ฝังอยู่ในไฟล์ตั้งค่า instance จริง:

```json
{
  "$schema": "https://cdn.neko-launcher.com/schema/neko-launcher.json",
  "name": "alice-magic",
  "displayName": "Alice Magic",
  "socials": [
    { "type": "discord", "url": "https://discord.gg/alicemagic" },
    { "type": "github", "url": "https://github.com/alice-magic" },
    { "type": "store", "url": "https://shop.furi.moe" },
    { "type": "youtube", "url": "https://youtube.com/@alicemagic" },
    { "type": "support", "url": "https://furipay.me/aomkoyo" },
    { "type": "website", "url": "https://furi.moe" },
    { "type": "website", "url": "https://neko-launcher.com/wiki", "label": "Wiki" },
    { "type": "iframe", "url": "https://www.youtube.com/embed/dQw4w9WgXcQ", "label": "Trailer" },
    { "type": "telegram", "url": "https://t.me/alicemagic" }
  ]
}
```

---

## ✅ แนวทางปฏิบัติที่ดี

**ลำดับและความสำคัญ**

* วางลิงก์ที่สำคัญที่สุดไว้ก่อน โดยปกติคือ Discord หรือศูนย์กลางชุมชนหลักของคุณ
* จัดกลุ่มแพลตฟอร์มที่เกี่ยวข้องกันไว้ด้วยกัน (โซเชียลมีเดียทั้งหมด ตามด้วยการพัฒนา แล้วจึงร้านค้า)
* จำกัดให้อยู่ที่ประมาณ 3–7 ลิงก์ เพื่อให้แถวลิงก์ยังอ่านง่าย

**URL**

* ใช้ `https://` ทุกที่
* ชี้ไปยัง URL อย่างเป็นทางการและแบบมาตรฐาน และยืนยันว่าเข้าถึงได้แบบสาธารณะ
* ทดสอบทุกลิงก์ก่อนเผยแพร่ instance

**การเลือกแพลตฟอร์ม**

* เพิ่มเฉพาะแพลตฟอร์มที่ชุมชนของคุณใช้งานอยู่จริง อย่าเพิ่มลิงก์เพียงเพื่อความครบถ้วน
* คอยอัปเดตให้เป็นปัจจุบัน อัปเดตหรือลบรายการออกเมื่อแพลตฟอร์มเปลี่ยนแปลงหรือปิดตัวลง

---

## ดูเพิ่มเติม

* [Instance Configuration](instance-configuration.md) — อ้างอิง `instance.json` ฉบับเต็ม
* [Announcement Instance](announcement-instance.md) — โปรโมตข่าวสารและกิจกรรมภายใน launcher
* [Documentation Index](README.md)
