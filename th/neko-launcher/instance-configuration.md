# Instance Configuration Schema

Schema นี้ใช้สำหรับตั้งค่า **Minecraft instance ใน Neko Launcher**

* **$comment**: Schema สำหรับตั้งค่า Minecraft instance
* **title**: `Neko Launcher Instance Schema`
* **type**: `object`

---

## ฟิลด์ที่จำเป็น

| ฟิลด์         | ชนิดข้อมูล | คำอธิบาย                                                                 |
| ------------- | ---------- | ------------------------------------------------------------------------ |
| `name`        | string     | รหัสเฉพาะของ instance (ตัวอักษรเล็ก, ตัวเลข, ขีดกลาง, ขีดล่าง)        |
| `displayName` | string     | ชื่อที่แสดงของ instance                                                 |
| `description` | string     | คำอธิบายสั้น ๆ ของ instance                                             |
| `onlineMode`  | boolean    | ต้องการตรวจสอบตัวตนออนไลน์หรือไม่                                       |
| `minecraft`   | object     | การตั้งค่าเวอร์ชัน Minecraft และ mod loader                             |

---

## ฟิลด์เพิ่มเติม

| ฟิลด์                     | ชนิดข้อมูล   | คำอธิบาย                                         |
| ------------------------- | ------------ | ------------------------------------------------ |
| `icon`                    | string (URI) | URL รูปไอคอนของ instance                        |
| `minecraft.version`       | string       | เวอร์ชัน Minecraft เช่น `1.21.8`, `latest`      |
| `minecraft.loader.type`   | string       | ประเภท loader (`fabric`, `forge`, `quilt`, `neoforge`) |
| `minecraft.loader.build`  | string       | เวอร์ชันของ loader                              |
| `minecraft.loader.enable` | boolean      | เปิด/ปิด mod loader                              |
| `metadata.wallpaper`      | string (URI) | URL รูป wallpaper                                |
| `metadata.[localized]`    | string       | ฟิลด์แปลภาษา เช่น `th_displayName`, `th_description` |
| `tags`                    | string[]     | แท็กสำหรับจัดหมวดหมู่ instance                  |
| `ignored`                 | string[]     | ไฟล์หรือโฟลเดอร์ที่ไม่ต้อง sync/packaging       |
| `readonly`                | boolean      | กำหนดให้อ่านอย่างเดียว                          |
| `gameArgs`                | string[]     | อาร์กิวเมนต์ JVM/เกมเพิ่มเติม                   |
| `socials`                 | array        | ลิงก์โซเชียล/ชุมชน/ร้านค้า                     |

---

## ตัวอย่างการตั้งค่า Minecraft

```json
"minecraft": {
  "version": "1.21.8",
  "loader": {
    "type": "fabric",
    "build": "0.17.2",
    "enable": true
  }
}
```

### ฟิลด์เวอร์ชัน
* ใช้เวอร์ชันเฉพาะ เช่น `1.21.8`, `1.20.1`
* หรือใช้ `latest` สำหรับเวอร์ชันล่าสุด

### ประเภท Loader
* `fabric` - Fabric Loader
* `forge` - Forge Mod Loader
* `quilt` - Quilt Loader
* `neoforge` - NeoForge Loader

---

## ข้อมูล metadata และการแปลภาษา

```json
"metadata": {
  "wallpaper": "https://cdn.example.com/wallpaper.webp",
  "th_displayName": "ชื่อภาษาไทย",
  "th_description": "คำอธิบายภาษาไทย",
  "customField": "ข้อมูลเพิ่มเติม"
}
```

ฟิลด์แปลภาษาใช้รูปแบบ `{locale}_{field}` เช่น `th_displayName`, `ja_description`

---

## การใช้ tags

```json
"tags": [
  "Survival",
  "Modded",
  "RPG",
  "Community"
]
```

ดูรายการ tag ที่รองรับในเนื้อหาภาษาอังกฤษด้านล่าง

---

## การตั้งค่า ignored

```json
"ignored": [
  "logs",
  "crash-reports",
  "screenshots",
  "*.log",
  "saves/*/playerdata"
]
```

รองรับ path ตรง, glob pattern, และ path ซ้อน

---

## การตั้งค่า gameArgs

```json
"gameArgs": [
  "--quickPlayMultiplayer=play.furi.moe",
  "-Xmx4G",
  "-XX:+UseG1GC"
]
```

---

## ตัวอย่างเต็ม

ดูตัวอย่างเต็มในเนื้อหาภาษาอังกฤษด้านล่าง

---

## ดูเพิ่มเติม

* [ไฟล์ Manifest](instance-manifest.md) - กำหนดไฟล์ดาวน์โหลด
* [ลิงก์โซเชียล](social-links.md) - ตั้งค่าชุมชน
* [กลับสู่หน้ารวมเอกสาร](README.md)

---

# Instance Configuration Schema (English)

Schema สำหรับการตั้งค่า **Neko Launcher Minecraft instance**

* **$comment**: Schema for Neko Launcher Minecraft instance configuration
* **title**: `Neko Launcher Instance Schema`
* **type**: `object`

---

## Required Fields

| Field         | Type    | Description                                                                                      |
| ------------- | ------- | ------------------------------------------------------------------------------------------------ |
| `name`        | string  | ตัวระบุเฉพาะของ instance (ตัวอักษรพิมพ์เล็ก, ตัวเลข, ขีดกลาง, ขีดล่าง)                            |
| `displayName` | string  | ชื่อที่แสดงของ instance                                                                           |
| `description` | string  | คำอธิบายสั้นๆ ของ instance                                                                        |
| `onlineMode`  | boolean | กำหนดว่า instance ต้องการการยืนยันตัวตนออนไลน์หรือไม่                                             |
| `minecraft`   | object  | การตั้งค่าเวอร์ชัน Minecraft และ loader                                                           |

---

## Properties

| Field                     | Type         | Description                                                              |
| ------------------------- | ------------ | ------------------------------------------------------------------------ |
| `icon`                    | string (URI) | URL ของไอคอน instance                                                     |
| `minecraft.version`       | string       | เวอร์ชัน Minecraft (เช่น `1.21.8`, `latest`)                             |
| `minecraft.loader.type`   | string       | ประเภท Loader (`fabric`, `forge`, `quilt`, `neoforge`)                    |
| `minecraft.loader.build`  | string       | เวอร์ชัน build ของ Loader                                                |
| `minecraft.loader.enable` | boolean      | เปิดหรือปิดการใช้งาน mod loader                                           |
| `metadata.wallpaper`      | string (URI) | URL ของภาพพื้นหลัง                                                        |
| `metadata.[localized]`    | string       | ฟิลด์ที่แปลเป็นภาษาท้องถิ่น เช่น `th_displayName`, `th_description`      |
| `ignored`                 | string[]     | ไฟล์หรือโฟลเดอร์ที่จะถูกละเว้นในระหว่างการเปิดหรือแพ็คเกจ                 |
| `readonly`                | boolean      | กำหนดว่า instance เป็นแบบอ่านอย่างเดียวหรือไม่                             |
| `gameArgs`                | string[]     | อาร์กิวเมนต์ JVM / เกมเพิ่มเติม                                           |
| `socials`                 | array        | ลิงก์โซเชียล / ชุมชน / ร้านค้าสำหรับ instance                             |
| `tags`                    | string[]     | แท็กที่ใช้จัดหมวดหมู่ประเภท/คุณสมบัติของ instance                        |

---

## Minecraft Version Configuration

Object `minecraft` กำหนดเวอร์ชันเกมและการตั้งค่า mod loader:

```json
"minecraft": {
  "version": "1.21.8",
  "loader": {
    "type": "fabric",
    "build": "0.17.2",
    "enable": true
  }
}
```

### Version Field
* ใช้เวอร์ชันที่เฉพาะเจาะจง เช่น `1.21.8`, `1.20.1`
* หรือใช้ `latest` สำหรับเวอร์ชันล่าสุด

### Loader Types
* `fabric` - Fabric Loader
* `forge` - Forge Mod Loader
* `quilt` - Quilt Loader
* `neoforge` - NeoForge Loader

---

## Metadata & Localization

Object `metadata` รองรับฟิลด์กำหนดเองรวมถึงเนื้อหาที่แปลเป็นภาษาท้องถิ่น:

```json
"metadata": {
  "wallpaper": "https://cdn.example.com/wallpaper.webp",
  "th_displayName": "ชื่อภาษาไทย",
  "th_description": "คำอธิบายภาษาไทย",
  "customField": "any custom metadata"
}
```

ฟิลด์ที่แปลเป็นภาษาท้องถิ่นใช้รูปแบบ `{locale}_{field}` โดยที่:
* `locale` คือรหัสภาษา (เช่น `th`, `ja`, `zh`)
* `field` คือ property ที่กำลังถูกแปล (โดยปกติจะเป็น `displayName` หรือ `description`)

---

## Tags

ใช้ array `tags` เพื่อจัดหมวดหมู่ instance ของคุณตามประเภทการเล่น คุณสมบัติ หรือสไตล์:

```json
"tags": [
  "Survival",
  "Modded",
  "RPG",
  "Community"
]
```

### แท็กที่รองรับ

**โหมดเกม:**
* `Survival` - การเล่นแบบเอาชีวิตรอดคลาสสิก
* `SMP` - Survival Multiplayer
* `Creative` - เน้นโหมด Creative
* `Hardcore` - ความยาก Hardcore
* `Adventure` - การเล่นโหมด Adventure
* `Spectator` - เปิดใช้งานโหมด Spectator

**ประเภทเซิร์ฟเวอร์:**
* `Skyblock` - การเล่น Skyblock
* `Prison` - สไตล์เซิร์ฟเวอร์คุก
* `Factions` - สงคราม Factions
* `Earth` - เซิร์ฟเวอร์แผนที่โลก
* `Anarchy` - การเล่นแบบ Anarchy
* `Lifesteal` - กลไก Lifesteal
* `Pixelmon` - มอด Pixelmon
* `Cobblemon` - มอด Cobblemon

**มินิเกม:**
* `Minigame` - มินิเกมทั่วไป
* `BedWars` - มินิเกม BedWars
* `SkyWars` - มินิเกม SkyWars
* `Parkour` - ความท้าทาย Parkour
* `PvP` - เน้นการต่อสู้ผู้เล่นกับผู้เล่น

**สไตล์การเล่น:**
* `RPG` - องค์ประกอบเกมสวมบทบาท
* `MMO` - เกมออนไลน์ผู้เล่นหมู่มาก
* `Magic` - ระบบเวทมนตร์/คาถา
* `Dungeons` - การสำรวจดันเจี้ยน
* `Quests` - ระบบเควส
* `Lore` - เน้นเรื่องราว/ตำนาน

**ด้านเทคนิค:**
* `Modded` - รวมมอด
* `Vanilla` - Minecraft แท้
* `Crossplay` - เล่นข้าม Bedrock/Java
* `Voice Chat` - ผสานระบบพูดคุยด้วยเสียง
* `Custom Items` - ไอเทม/ทรัพยากรกำหนดเอง
* `Tech` - มอดเทคโนโลยี/อัตโนมัติ
* `Redstone` - เน้น Redstone

**ชุมชน:**
* `Economy` - ระบบเศรษฐกิจ
* `Roleplay` - มุ่งเน้นการเล่นสวมบทบาท
* `Community` - เน้นชุมชน
* `Events` - อีเวนต์ประจำ
* `Building` - เน้นการก่อสร้าง
* `Towny` - ปลั๊กอิน/มอด Towny

---

## Ignored Files

ใช้ array `ignored` เพื่อยกเว้นไฟล์/โฟลเดอร์จากการซิงค์หรือแพ็คเกจ:

```json
"ignored": [
  "logs",
  "crash-reports",
  "screenshots",
  "*.log",
  "saves/*/playerdata"
]
```

รองรับ:
* path ที่แน่นอน: `logs`, `screenshots`
* รูปแบบ Glob: `*.log`, `crash-reports/*.txt`
* path ที่ซ้อนกัน: `saves/world/region`

---

## Game Arguments

เพิ่มอาร์กิวเมนต์ JVM หรือเกมแบบกำหนดเองผ่าน array `gameArgs`:

```json
"gameArgs": [
  "--quickPlayMultiplayer=play.furi.moe",
  "-Xmx4G",
  "-XX:+UseG1GC"
]
```

กระบวนการใช้งานทั่วไป:
* เชื่อมต่อเซิร์ฟเวอร์อัตโนมัติ: `--quickPlayMultiplayer=server.address`
* จัดสรรหน่วยความจำ: `-Xmx4G`, `-Xms2G`
* แฟล็ก JVM: `-XX:+UseG1GC`

---

## ตัวอย่างที่สมบูรณ์

```json
{
  "$schema": "https://cdn.furimoe.com/schema/neko-launcher.json",
  "name": "alice-magic",
  "displayName": "Alice Magic: Furiora's World",
  "description": "A modded Minecraft experience with adventure.",
  "icon": "https://cdn.masuru.in.th/HamF2Hsoirx4K9DL.webp",
  "onlineMode": true,

  "minecraft": {
    "version": "1.21.8",
    "loader": {
      "type": "fabric",
      "build": "0.17.2",
      "enable": true
    }
  },

  "metadata": {
    "th_displayName": "อลิซ เมจิก: โลกของฟูริโอร่า",
    "th_description": "ประสบการณ์มอด Minecraft ที่เต็มไปด้วยการผจญภัย",
    "wallpaper": "https://cdn.masuru.in.th/2025-11-18_13.webp"
  },

  "tags": [
    "Survival",
    "Modded",
    "RPG",
    "Magic",
    "Community"
  ],

  "socials": [
    { "type": "github", "url": "https://github.com/aomkoyo" },
    { "type": "store", "url": "https://shop.furi.moe" },
    { "type": "support", "url": "https://furipay.me/aomkoyo" }
  ],

  "ignored": [
    "logs",
    "crash-reports",
    "screenshots"
  ],

  "readonly": false,
  "gameArgs": [
    "--quickPlayMultiplayer=play.furi.moe"
  ]
}
```

---

## ดูเพิ่มเติม

* [Instance Manifest](instance-manifest.md) - กำหนดไฟล์ที่สามารถดาวน์โหลดได้สำหรับ instance ของคุณ
* [Social Links](social-links.md) - ตั้งค่าลิงก์ชุมชนและโซเชียล
* [กลับไปยังสารบัญเอกสาร](README.md)
