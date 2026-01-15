[ดาวน์โหลด Neko Launcher](https://launcher.furi.moe/en) | [Furimoe](https://furi.moe)

# เอกสาร Neko Launcher

ยินดีต้อนรับสู่เอกสารประกอบของ **Neko Launcher** คู่มือนี้ครอบคลุม JSON schemas ตัวเลือกการกำหนดค่า และข้อมูลจำเพาะทางเทคนิคสำหรับการสร้างและจัดการอินสแตนซ์ Minecraft ด้วย Neko Launcher

---

## ภาพรวมเอกสาร

### Core Schemas

* **[การกำหนดค่าอินสแตนซ์](instance-configuration)** - สกีมาที่สมบูรณ์สำหรับการกำหนดค่าอินสแตนซ์ Minecraft รวมถึงการตั้งค่าเวอร์ชัน เมตาดาต้า และอาร์กิวเมนต์เกม
* **[Instance Manifest](instance-manifest)** - สกีมาสำหรับกำหนดทรัพยากรที่ดาวน์โหลดได้พร้อมการตรวจสอบความสมบูรณ์
* **[ลิงก์โซเชียล](social-links)** - กำหนดค่าลิงก์ชุมชน การพัฒนา และร้านค้าสำหรับอินสแตนซ์ของคุณ

### การผสานรวมและการค้นหา

* **[การค้นหาผ่าน DNS](dns-discovery)** - การกำหนดค่าการค้นหาอัตโนมัติโดยใช้ DNS TXT records
* **[การตรวจสอบ HTTP Header](http-headers)** - ส่วนหัวการตรวจสอบสิทธิ์และการยืนยันที่ส่งโดยตัวเปิด

---

## เริ่มต้นอย่างรวดเร็ว

1. สร้างไฟล์ `instance.json` ตาม [Instance Configuration Schema](instance-configuration)
2. สร้างไฟล์ `manifest.json` พร้อมไฟล์อินสแตนซ์ของคุณโดยใช้ [Manifest Schema](instance-manifest)
3. (ตัวเลือก) กำหนดค่า [Social Links](social-links) สำหรับการมีส่วนร่วมของชุมชน
4. (ตัวเลือก) ตั้งค่า [DNS Discovery](dns-discovery) สำหรับการตรวจจับอินสแตนซ์อัตโนมัติ

---

## อ้างอิง Schema

Schema หลักมีอยู่ที่:
```text
https://cdn.furimoe.com/schema/neko-launcher.json
```

รวมไว้ในการกำหนดค่าของคุณ:
```json
{
  "$schema": "https://cdn.furimoe.com/schema/neko-launcher.json",
  "name": "my-instance",
  ...
}
```

---

## การสนับสนุนและทรัพยากร

* [GitHub](https://github.com/alice-magic)
* [ดาวน์โหลด Launcher](https://launcher.furi.moe/en)
* [เว็บไซต์หลัก](https://furi.moe)
