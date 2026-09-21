# ลิงก์โซเชียล

`socials` ใส่ปุ่มใต้คำอธิบายอินสแตนซ์: ชุมชน โค้ด ร้านค้า เว็บไซต์ วิดีโอฝัง handle สำหรับบริจาค ตัวเปิดเลือกไอคอนและสีจาก `type`

---

## รูปร่าง

```json
{
  "socials": [
    { "type": "discord", "url": "https://discord.gg/example" },
    { "type": "youtube", "url": "https://youtube.com/@example" },
    { "type": "iframe", "url": "https://www.youtube.com/watch?v=uTBy-PKrH3w" },
    { "type": "web", "url": "https://example.com", "label": "เว็บไซต์" },
    { "type": "furipay", "url": "aomkoyo" }
  ]
}
```

| ฟิลด์ | บังคับ | หมายเหตุ |
|---|---|---|
| `type` | ใช่ | หนึ่งใน key ด้านล่าง type ที่ไม่รู้จักแสดงแบบกลางๆ |
| `url` | ใช่ | ลิงก์ สำหรับ `furipay` คือ FuriPay handle ไม่ใช่ URL |
| `label` | ไม่ | ข้อความกำหนดเอง ใช้กับ `web`/`website` และ `iframe` |

ทั้งฟิลด์ไม่บังคับ เจ้าของบนแดชบอร์ดแก้ในการตั้งค่าอินสแตนซ์

## ประเภท

| กลุ่ม | Key | แสดงเป็น |
|---|---|---|
| ชุมชน | `discord`, `dc`, `telegram`, `tg` | ไอคอนและสีของแบรนด์ |
| โค้ด | `github`, `gh`, `gitlab` | ไอคอนแบรนด์ |
| โซเชียล | `facebook`, `fb`, `youtube`, `yt`, `x`, `twitter`, `instagram`, `ig`, `tiktok`, `twitch` | ไอคอนแบรนด์ |
| ร้านค้า | `store`, `shop`, `market` | ไอคอนร้าน |
| สนับสนุน | `patreon`, `kofi`, `support` | ไอคอนหัวใจ |
| เว็บไซต์ | `web`, `website` | ไอคอนลูกโลก แสดง `label` |
| ฝัง | `iframe` | เปิด URL ใน modal ในตัวเปิด (ลิงก์ YouTube ฝังเป็นเครื่องเล่น) |
| บริจาค | `furipay` | เปิดหน้า FuriPay ของ handle ใน `url` |
| อื่นๆ | `other` | ไอคอนลิงก์ทั่วไป |

## ดูเพิ่มเติม

- [การตั้งค่าอินสแตนซ์](instance-configuration.md)
