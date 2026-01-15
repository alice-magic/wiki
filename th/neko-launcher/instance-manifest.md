# Instance Manifest Schema

Instance manifest กำหนดไฟล์ทั้งหมดที่จำเป็นในการเปิด Minecraft instance รวมถึง mods, resource packs, การตั้งค่า และไฟล์อื่นๆ

* **type**: `array`
* **description**: รายการไฟล์ที่สามารถดาวน์โหลดได้พร้อมข้อมูลความถูกต้อง

---

## Manifest Structure

Manifest เป็น JSON array ที่ประกอบด้วย object ของไฟล์:

```json
[
  {
    "path": "mods/example-mod.jar",
    "url": "https://cdn.example.com/mod.jar",
    "size": 1234567,
    "hash": "abc123def456..."
  }
]
```

---

## Item Fields

| Field  | Type         | Required | Description                                       |
| ------ | ------------ | -------- | ------------------------------------------------- |
| `path` | string       | ✓        | path ของไฟล์แบบสัมพัทธ์ใน instance directory      |
| `url`  | string (URI) | ✓        | URL สำหรับดาวน์โหลด                                |
| `size` | integer      | ✓        | ขนาดไฟล์เป็นไบต์                                  |
| `hash` | string       | ✓        | SHA-1 hash สำหรับการตรวจสอบความถูกต้อง            |

---

## Path Guidelines

ฟิลด์ `path` กำหนดตำแหน่งที่ไฟล์จะถูกวางภายใน instance:

### Common Directories

* `mods/` - ไฟล์ JAR ของ Mod
* `resourcepacks/` - Resource packs
* `shaderpacks/` - Shader packs
* `config/` - ไฟล์การตั้งค่า Mod
* `kubejs/` - สคริปต์ KubeJS
* `defaultconfigs/` - การตั้งค่าเริ่มต้นของ Mod
* `scripts/` - CraftTweaker หรือสคริปต์อื่นๆ

### Path Rules

* ใช้เครื่องหมายทับไปข้างหน้า `/` (ไม่ใช่แบ็กสแลช)
* Path เป็นแบบสัมพัทธ์กับรากของ instance
* ไม่มีเครื่องหมายทับนำหน้า
* รักษาชื่อไฟล์เดิมไว้ตามความเป็นไปได้

---

## Hash Verification

ฟิลด์ `hash` ใช้ **SHA-1** สำหรับความถูกต้องของไฟล์:

* ตรวจสอบให้แน่ใจว่าไฟล์ที่ดาวน์โหลดตรงกับเนื้อหาที่คาดหวัง
* ป้องกันความเสียหายระหว่างการดาวน์โหลด
* ยืนยันความถูกต้องของไฟล์

### Generate SHA-1 Hash

**Linux/macOS:**
```bash
sha1sum file.jar
```

**Windows (PowerShell):**
```powershell
Get-FileHash -Algorithm SHA1 file.jar
```

**Node.js:**
```javascript
const crypto = require('crypto');
const fs = require('fs');

const hash = crypto.createHash('sha1');
hash.update(fs.readFileSync('file.jar'));
// console.log(hash.digest('hex'));
```

---

## Complete Example

```json
[
  {
    "path": "mods/iris-fabric-1.9.2+mc1.21.8.jar",
    "url": "https://cdn.modrinth.com/data/YL57xq9U/versions/x2f4KxP0/iris-fabric-1.9.2%2Bmc1.21.8.jar",
    "size": 2741900,
    "hash": "4964121fd2d75eebec77dd8b723bc381952fe43a"
  },
  {
    "path": "mods/sodium-fabric-0.6.5+mc1.21.8.jar",
    "url": "https://cdn.modrinth.com/data/AANobbMI/versions/b2fzXn3C/sodium-fabric-0.6.5%2Bmc1.21.8.jar",
    "size": 891234,
    "hash": "9876543210abcdef9876543210abcdef98765432"
  },
  {
    "path": "resourcepacks/Faithful 64x - Release 10.zip",
    "url": "https://cdn.modrinth.com/data/r4GILswZ/versions/5T6GekBK/Faithful%2064x%20-%20Release%2010.zip",
    "size": 18001871,
    "hash": "00413cf363e708c93a11f87dd425f0a164f3c1fb"
  },
  {
    "path": "config/iris.properties",
    "url": "https://cdn.example.com/config/iris.properties",
    "size": 2048,
    "hash": "abcdef1234567890abcdef1234567890abcdef12"
  }
]
```

---

## Best Practices

### File Organization

* จัดกลุ่มไฟล์ที่เกี่ยวข้อง (mods ทั้งหมดไว้ด้วยกัน, configs ไว้ด้วยกัน)
* รักษาหลักการตั้งชื่อที่สอดคล้องกัน
* ใช้ชื่อไฟล์ที่อธิบายได้ชัดเจน

### URL Management

* ใช้ CDN เพื่อความเร็วในการดาวน์โหลดที่ดีขึ้น
* พิจารณาการเข้ารหัส URL สำหรับอักขระพิเศษ
* ตรวจสอบว่า URL สามารถเข้าถึงได้สาธารณะ
* ทดสอบ URL ทั้งหมดก่อนการ deploy

### Version Control

* อัปเดต manifest เมื่อมีการเพิ่ม/ลบไฟล์
* บันทึกการเปลี่ยนแปลงใน version control
* ทดสอบ manifest ก่อนการเผยแพร่

### Performance

* ลดจำนวนไฟล์ทั้งหมดให้น้อยที่สุดตามความเป็นไปได้
* รวมไฟล์ขนาดเล็กเข้าด้วยกันเมื่อเหมาะสม
* ใช้รูปแบบที่บีบอัด (ZIP, JAR)
* พิจารณาผลกระทบของขนาดไฟล์ต่อเวลาดาวน์โหลด

---

## Validation

ก่อนการ deploy ให้ตรวจสอบ manifest ของคุณ:

1. **ตรวจสอบ URL ทั้งหมด** - ตรวจสอบว่าไฟล์สามารถดาวน์โหลดได้
2. **ยืนยัน hash** - ยืนยันว่า SHA-1 ตรงกับไฟล์จริง
3. **ตรวจสอบ path** - ตรวจสอบโครงสร้างไดเรกทอรีที่ถูกต้อง
4. **ทดสอบขนาด** - ตรวจสอบว่าค่าขนาดถูกต้อง
5. **ไวยากรณ์ JSON** - ตรวจสอบรูปแบบ JSON

---

## See Also

* [Instance Configuration](instance-configuration.md) - Schema การตั้งค่า instance หลัก
* [HTTP Headers](http-headers.md) - Header สำหรับการยืนยันตัวตนในการดาวน์โหลด
* [กลับไปยังสารบัญเอกสาร](README.md)
