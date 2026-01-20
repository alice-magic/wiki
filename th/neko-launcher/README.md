# เอกสาร Neko Launcher (ภาษาไทย)

ยินดีต้อนรับสู่เอกสาร **Neko Launcher** คู่มือนี้ครอบคลุมโครงสร้าง JSON, ตัวเลือกการตั้งค่า และรายละเอียดทางเทคนิคสำหรับการสร้างและจัดการ Minecraft instance ด้วย Neko Launcher

---

## สารบัญ

### สคีมาและการตั้งค่าหลัก

* **[การตั้งค่า Instance](instance-configuration.md)** - สคีมาครบถ้วนสำหรับการตั้งค่า Minecraft instance เช่น เวอร์ชัน, ข้อมูล, และอาร์กิวเมนต์เกม
* **[ไฟล์ Manifest](instance-manifest.md)** - สคีมาสำหรับกำหนดไฟล์ดาวน์โหลดและตรวจสอบความถูกต้อง
* **[ลิงก์โซเชียล](social-links.md)** - ตั้งค่าชุมชน, การพัฒนา, และร้านค้า

### การเชื่อมต่อและค้นหา

* **[ค้นหาอัตโนมัติผ่าน DNS](dns-discovery.md)** - ตั้งค่าการค้นหา instance อัตโนมัติด้วย DNS TXT record
* **[ตรวจสอบ HTTP Header](http-headers.md)** - การตรวจสอบและยืนยันตัวตนผ่าน header

---

## เริ่มต้นอย่างรวดเร็ว

1. สร้างไฟล์ `instance.json` ตาม [สคีมาตั้งค่า Instance](instance-configuration.md)
2. สร้างไฟล์ `manifest.json` ด้วยไฟล์ instance โดยใช้ [สคีมา Manifest](instance-manifest.md)
3. (เลือก) ตั้งค่า [ลิงก์โซเชียล](social-links.md) เพื่อสร้างชุมชน
4. (เลือก) ตั้งค่า [DNS Discovery](dns-discovery.md) เพื่อค้นหา instance อัตโนมัติ

---

## อ้างอิงสคีมา

สคีมาหลักสามารถดูได้ที่:
```text
https://cdn.furimoe.com/schema/neko-launcher.json
```

ตัวอย่างการใช้งานในไฟล์ config:
```json
{
  "$schema": "https://cdn.furimoe.com/schema/neko-launcher.json",
  "name": "my-instance",
  ...
}
```

---

## การประกาศ (Instance Announcement)

สามารถแสดงประกาศในหน้า instance ได้โดยตั้งค่า `announcementUrl` ในไฟล์ config โดย URL นี้ต้องชี้ไปยังไฟล์ JSON ที่มี array ของประกาศ

ตัวอย่างการตั้งค่า:
```json
{
  "name": "my-instance",
  "announcementUrl": "https://example.com/announcement.json"
}
```

ตัวอย่างโครงสร้าง JSON:
```json
{
  "title": "แจ้งเตือนบำรุงระบบ",
  "category": "NOTICE", // NOTICE | NEWS | EVENT
  "metadata": {
    "imageUrl": "https://example.com/image.png",
    "th_imageUrl": "https://example.com/image-th.png"
  },
  "link": "https://example.com/details",
  "active": true,
  "date": "2026-01-20T12:00:00Z"
}
```

> ตัวอย่างนี้ใช้เพื่อแสดงรูปแบบเท่านั้น กรุณาปรับข้อมูลให้เหมาะสมกับ instance ของคุณ

---

## แหล่งข้อมูลและการสนับสนุน

* [GitHub](https://github.com/alice-magic)
* [ดาวน์โหลด Launcher](https://launcher.furi.moe/en)
* [เว็บไซต์หลัก](https://furi.moe)

---

# Neko Launcher Documentation (English)

Neko Launcher is an alternative launcher for players who are unable to use MagicLauncher.

---

## General Information

### Supported Operating Systems
- 🪟 **Windows**
- 🍎 **macOS**
- 🐧 **Linux**

### Download
📥 [Download Neko Launcher](https://launcher.furi.moe/th)

---

## How to Use

Follow these steps to start playing:

1. **Open Neko Launcher** and click **Login**
2. **Click Search** (ค้นหา)
3. **Type** `play.furi.moe`
4. **Click Play** เพื่อเริ่มเกม

> 💡 Additional Info: If you have trouble connecting, please refer to the guide on [Joining with IP Address](../how-to/join-with-ip-address.md)

---

## Technical Documentation

Welcome to the **Neko Launcher** documentation. This guide covers JSON schemas, configuration options, and technical specifications for creating and managing Minecraft instances with Neko Launcher.

---

## Document Overview

### Core Schemas

* **[Instance Configuration](instance-configuration)** - A complete schema for configuring Minecraft instances, including version settings, metadata, and game arguments.
* **[Instance Manifest](instance-manifest)** - A schema for defining downloadable resources with integrity checks.
* **[Social Links](social-links)** - Configure community, development, and store links for your instance.

### Integration and Discovery

* **[DNS Discovery](dns-discovery)** - Configuring automatic discovery using DNS TXT records.
* **[HTTP Header Verification](http-headers)** - Authentication and verification headers sent by the launcher.

---

## Quick Start

1. Create an `instance.json` file according to the [Instance Configuration Schema](instance-configuration)
2. Create a `manifest.json` file with your instance files using the [Manifest Schema](instance-manifest)
3. (Optional) Configure [Social Links](social-links) for community engagement
4. (Optional) Set up [DNS Discovery](dns-discovery) for automatic instance detection

---

## Schema Reference

The main schema is available at:
```text
https://cdn.furimoe.com/schema/neko-launcher.json
```

Include it in your configuration:
```json
{
  "$schema": "https://cdn.furimoe.com/schema/neko-launcher.json",
  "name": "my-instance",
  ...
}
```

---

## Support and Resources

* [GitHub](https://github.com/alice-magic)
* [Download Launcher](https://launcher.furi.moe/en)
* [Main Website](https://furi.moe)
