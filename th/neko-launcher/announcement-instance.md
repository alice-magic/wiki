# การใช้งาน Instance Announcement

Neko Launcher รองรับการแสดงประกาศสำหรับแต่ละ instance โดยใช้ property `announcementUrl` ในไฟล์ configuration ของ instance คุณ สามารถแจ้งข่าวสาร กิจกรรม หรือประกาศสำคัญให้ผู้เล่นเห็นได้โดยตรงใน launcher

---

## ภาพรวม

ฟีเจอร์ประกาศนี้ช่วยให้คุณ:
* **แจ้งเตือนในแอป** - แสดงข่าวสาร กิจกรรม และประกาศให้ผู้เล่น
* **รองรับหลายภาษา** - แสดงเนื้อหาและรูปภาพที่แปลได้
* **ขยายข้อมูลได้อิสระ** - เพิ่ม field ใน metadata ตามต้องการ

---

## การตั้งค่า

เพิ่ม `announcementUrl` ในไฟล์ configuration ของ instance:

```json
{
  "name": "my-instance",
  "announcementUrl": "https://example.com/announcement-instance.json"
}
```

URL นี้ต้องชี้ไปยังไฟล์ JSON ที่เข้าถึงได้จากภายนอก และมีข้อมูลประกาศเป็น array

---

## รูปแบบ JSON ของประกาศ

แต่ละประกาศควรมีโครงสร้างดังนี้:

| ฟิลด์        | ชนิดข้อมูล | คำอธิบาย                                   |
|--------------|------------|---------------------------------------------|
| title        | string     | หัวข้อประกาศ                               |
| category     | string     | 'NOTICE', 'NEWS', หรือ 'EVENT'              |
| link         | string     | ลิงก์สำหรับรายละเอียดเพิ่มเติม             |
| active       | boolean    | ประกาศนี้แสดงอยู่หรือไม่                   |
| date         | string     | วันที่และเวลา (ISO 8601)                   |
| metadata     | object     | ข้อมูลเพิ่มเติม เช่น รูปภาพ, คำแปล ฯลฯ     |

### ตัวอย่าง

> ตัวอย่างนี้ใช้เพื่อแสดงรูปแบบเท่านั้น กรุณาปรับข้อมูลให้เหมาะสมกับ instance ของคุณ

```json
[
  {
    "title": "แจ้งเตรียมบำรุงระบบ",
    "category": "NOTICE",
    "link": "https://status.furimoe.com",
    "active": true,
    "date": "2024-02-10T10:00:00.000Z",
    "metadata": {}
  },
  {
    "title": "พร้อมกลายเป็น Aha แล้วหรือยัง?",
    "category": "NEWS",
    "link": "https://www.hoyolab.com/article/43294281",
    "active": true,
    "date": "2026-01-17T09:30:00.000Z",
    "metadata": {
      "th_title": "พร้อมกลายเป็น Aha แล้วหรือยัง?",
      "imageUrl": "https://upload-os-bbs.hoyolab.com/upload/2026/01/16/a5db23dc248ab90458d95a2f94cbbf6a_5621172732061645560.png?x-oss-process=image%2Fresize%2Cs_1000%2Fauto-orient%2C0%2Finterlace%2C1%2Fformat%2Cwebp%2Fquality%2Cq_70",
      "th_imageUrl": "https://upload-os-bbs.hoyolab.com/upload/2026/01/16/e62a985320960b34870fc65ef24168eb_2821068148369816818.png?x-oss-process=image%2Fresize%2Cs_1000%2Fauto-orient%2C0%2Finterlace%2C1%2Fformat%2Cwebp%2Fquality%2Cq_70"
    }
  },
  {
    "title": "Minato Aqua Crismas Special",
    "category": "NEWS",
    "link": "https://www.youtube.com/watch?v=6bnaBnd4kyU",
    "active": true,
    "date": "2024-12-25T15:00:00.000Z",
    "metadata": {
      "imageUrl": "https://cdn.furimoe.com/images/114758380_p0_master1200.jpg"
    }
  }
]
```

---

## โครงสร้างข้อมูล (Type Definitions)

```typescript
export interface AnnouncementMetadata {
    imageUrl?: string;
    th_imageUrl?: string;
    [key: string]: any;
}

export interface Announcement {
    title: string;
    category: 'NOTICE' | 'NEWS' | 'EVENT';
    metadata: AnnouncementMetadata;
    link: string;
    active: boolean;
    date: Date;
}
```

---

## ข้อควรปฏิบัติ

* ใช้รูปแบบวันที่ ISO 8601 เช่น `2026-01-17T09:30:00.000Z`
* กำหนด `active: true` สำหรับประกาศที่ต้องการให้แสดง
* ใช้ `metadata` สำหรับข้อมูลแปลและรูปภาพ
* โฮสต์ไฟล์ JSON บน CDN หรือเว็บเซิร์ฟเวอร์ที่เชื่อถือได้
* ทดสอบ URL ให้เข้าถึงได้จากภายนอก

---

## การแก้ไขปัญหา

* ตรวจสอบว่า `announcementUrl` ถูกต้องและเข้าถึงได้
* ตรวจสอบรูปแบบ JSON ด้วยเครื่องมือออนไลน์
* ตรวจสอบ URL รูปภาพและข้อมูลแปลใน `metadata`
* ใช้ HTTPS สำหรับทุก resource

---

## ดูเพิ่มเติม

* [การตั้งค่า Instance](instance-configuration.md)
* [HTTP Headers](http-headers.md)
* [กลับสู่หน้ารวมเอกสาร](README.md)
