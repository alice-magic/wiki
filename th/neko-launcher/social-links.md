# Social Links Configuration

ฟิลด์ `socials` ช่วยให้ผู้สร้าง instance สามารถกำหนดลิงก์อย่างเป็นทางการสำหรับชุมชน, การพัฒนา, ร้านค้า และแหล่งข้อมูลอื่นๆ Neko Launcher จะแสดงลิงก์เหล่านี้โดยอัตโนมัติพร้อมไอคอนและสีที่เหมาะสม

---

## Overview

ลิงก์โซเชียลจะแสดงในรายละเอียดของ instance และให้การเข้าถึงอย่างรวดเร็วไปยัง:

* แพลตฟอร์มชุมชน (Discord, Telegram)
* Repository การพัฒนา (GitHub, GitLab)
* หน้าร้านค้า / บริจาค
* เว็บไซต์อย่างเป็นทางการ
* เนื้อหาแบบฝัง

---

## Schema Structure

```typescript
socials: {
  url: string;
  type: string;
}[];
```

### Fields

| Field            | Type         | Required | Description                                 |
| ---------------- | ------------ | -------- | ------------------------------------------- |
| `socials[].url`  | string (URI) | ✓        | URL เป้าหมาย                                |
| `socials[].type` | string       | ✓        | คีย์แพลตฟอร์มที่ใช้กำหนดไอคอนและสี          |

* `socials` เป็น **ตัวเลือก** ในการตั้งค่า instance
* Launcher จะจับคู่ไอคอน + สีจาก `type` โดยอัตโนมัติ
* ประเภทที่ไม่รู้จักจะใช้การจัดรูปแบบเริ่มต้น

---

## Basic Example

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

## Supported Platforms

### Store & Commerce

| Platform        | Type Keys                    | Color |
| --------------- | ---------------------------- | ----- |
| ร้านค้า / Shop  | `store`, `shop`, `market`    | Green |

**ตัวอย่าง:**
```json
{ "type": "store", "url": "https://shop.furi.moe" }
```

---

### Development Platforms

| Platform | Type Keys      | Color  |
| -------- | -------------- | ------ |
| GitHub   | `github`, `gh` | Purple |
| GitLab   | `gitlab`       | Orange |

**ตัวอย่าง:**
```json
{ "type": "github", "url": "https://github.com/aomkoyo" }
```

---

### Social Media

| Platform       | Type Keys           | Color    |
| -------------- | ------------------- | -------- |
| Facebook       | `facebook`, `fb`    | Blue     |
| YouTube        | `youtube`, `yt`     | Red      |
| X (Twitter)    | `x`, `twitter`      | Sky Blue |
| Instagram      | `instagram`, `ig`   | Pink     |
| TikTok         | `tiktok`            | Red/Pink |

**ตัวอย่าง:**
```json
{ "type": "youtube", "url": "https://youtube.com/@channel" }
```

---

### Gaming & Streaming

| Platform | Type Keys       | Color   |
| -------- | --------------- | ------- |
| Discord  | `discord`, `dc` | Blurple |
| Twitch   | `twitch`        | Purple  |

**ตัวอย่าง:**
```json
{ "type": "discord", "url": "https://discord.gg/alicemagic" }
```

---

### Chat Platforms

| Platform | Type Keys         | Color    |
| -------- | ----------------- | -------- |
| Telegram | `telegram`, `tg`  | Sky Blue |

**ตัวอย่าง:**
```json
{ "type": "telegram", "url": "https://t.me/alicemagic" }
```

---

### Support & Donations

| Platform | Type Keys                    | Color |
| -------- | ---------------------------- | ----- |
| บริจาค   | `patreon`, `kofi`, `support` | Coral |

**ตัวอย่าง:**
```json
{ "type": "support", "url": "https://furipay.me/aomkoyo" }
```

---

### General & Media

| Platform    | Type Keys        | Color          |
| ----------- | ---------------- | -------------- |
| เว็บไซต์    | `web`, `website` | Emerald        |
| Embed       | `iframe`         | Violet (Modal) |

**ตัวอย่าง:**
```json
{ "type": "website", "url": "https://furi.moe" }
{ "type": "iframe", "url": "https://docs.example.com" }
```

---

### Default Fallback

ประเภทที่ไม่รู้จักจะใช้การจัดรูปแบบสีม่วงเริ่มต้น:

```json
{ "type": "custom", "url": "https://example.com" }
```

---

## Complete Platform Reference

| Category | Platform        | Key / Type                               | Color          |
| -------- | --------------- | ---------------------------------------- | -------------- |
| ร้านค้า  | Store / Shop    | `store`, `shop`, `market`                | Green          |
| Dev      | GitHub          | `github`, `gh`                           | Purple         |
|          | GitLab          | `gitlab`                                 | Orange         |
| Social   | Facebook        | `facebook`, `fb`                         | Blue           |
|          | YouTube         | `youtube`, `yt`                          | Red            |
|          | X (Twitter)     | `x`, `twitter`                           | Sky Blue       |
|          | Instagram       | `instagram`, `ig`                        | Pink           |
|          | TikTok          | `tiktok`                                 | Red/Pink       |
| Gaming   | Discord         | `discord`, `dc`                          | Blurple        |
|          | Twitch          | `twitch`                                 | Purple         |
| Chat     | Telegram        | `telegram`, `tg`                         | Sky Blue       |
| Support  | บริจาค          | `patreon`, `kofi`, `support`             | Coral          |
| General  | เว็บไซต์        | `web`, `website`                         | Emerald        |
| Media    | Embed           | `iframe`                                 | Violet (Modal) |
| Default  | อื่นๆ           | *any*                                    | Violet         |

---

## Advanced Example

```json
{
  "$schema": "https://cdn.furimoe.com/schema/neko-launcher.json",
  "name": "alice-magic",
  "displayName": "Alice Magic",
  
  "socials": [
    {
      "type": "discord",
      "url": "https://discord.gg/alicemagic"
    },
    {
      "type": "github",
      "url": "https://github.com/alice-magic"
    },
    {
      "type": "store",
      "url": "https://shop.furi.moe"
    },
    {
      "type": "youtube",
      "url": "https://youtube.com/@alicemagic"
    },
    {
      "type": "support",
      "url": "https://furipay.me/aomkoyo"
    },
    {
      "type": "website",
      "url": "https://furi.moe"
    },
    {
      "type": "telegram",
      "url": "https://t.me/alicemagic"
    }
  ]
}
```

---

## Best Practices

### Order & Priority
* วางลิงก์ที่สำคัญที่สุดไว้ก่อน (โดยปกติคือ Discord/ชุมชน)
* จัดกลุ่มแพลตฟอร์มที่เกี่ยวข้องไว้ด้วยกัน
* จำกัดให้อยู่ที่ 3-7 ลิงก์เพื่อประสบการณ์ผู้ใช้ที่ดีขึ้น

### URL Validation
* ใช้ HTTPS สำหรับ URL ทั้งหมด
* ยืนยันว่าลิงก์สามารถเข้าถึงได้สาธารณะ
* ทดสอบลิงก์ก่อนการ deploy
* ใช้ URL อย่างเป็นทางการ/แบบมาตรฐาน

### Platform Selection
* เลือกแพลตฟอร์มที่ชุมชนของคุณใช้งานอยู่จริง
* อย่าเพิ่มลิงก์เพียงเพื่อความสมบูรณ์
* อัปเดตลิงก์เมื่อแพลตฟอร์มเปลี่ยนแปลง

---

## See Also

* [Instance Configuration](instance-configuration.md) - การตั้งค่า instance หลัก
* [กลับไปยังสารบัญเอกสาร](README.md)
