# Instance ในเครื่องและ modpack

**Instance ในเครื่อง** คือ instance ที่คุณสร้างเอง ใช้ Minecraft เวอร์ชันไหนก็ได้ mod loader อะไรก็ได้ หรือ modpack ใดก็ได้ เก็บอยู่บนเครื่องของคุณและเล่นได้โดยไม่ต้องพึ่งเซิร์ฟเวอร์ของ Neko มีตั้งแต่ **Neko Launcher 2.2.34**

มีสามวิธี ทั้งหมดเริ่มจากปุ่ม **+** ด้านล่างของแถบด้านข้าง

| แท็บ | ใช้ทำอะไร |
|---|---|
| **Browse** | ค้นหา modpack จาก Modrinth, CurseForge, FTB และ ATLauncher แล้วติดตั้งได้ในคลิกเดียว หน้าต่างจะเปิดที่แท็บนี้ |
| **Import** | ลากไฟล์ `.mrpack` ของ Modrinth หรือ `.zip` ของ CurseForge ที่มีอยู่แล้วเข้ามา |
| **Create** | เริ่มจากศูนย์ เลือกเวอร์ชัน Minecraft และ Vanilla, Fabric, Quilt, Forge หรือ NeoForge |

---

## ค้นหา modpack

![หน้าค้นหา modpack จาก Modrinth ในหน้าต่าง Add instance](https://neko-launcher.com/docs/local-instances/browse.webp)

1. เลือกแพลตฟอร์มในช่องแรก: **Modrinth**, **CurseForge**, **FTB** หรือ **ATLauncher**
2. พิมพ์ชื่อในช่องค้นหา หรือเว้นว่างไว้เพื่อดู modpack ยอดนิยม
3. **เรียงลำดับ** ตามความเกี่ยวข้อง ยอดดาวน์โหลด ผู้ติดตาม ใหม่ล่าสุด หรืออัปเดตล่าสุด (Modrinth และ CurseForge)
4. ใช้เลขหน้าที่มุมขวาบนเพื่อเลื่อนดูผลลัพธ์
5. กดที่ modpack เพื่อเลือกเวอร์ชัน

![หน้าเลือกเวอร์ชันของ modpack](https://neko-launcher.com/docs/local-instances/versions.webp)

หน้าเลือกเวอร์ชันแสดง modpack ทางซ้าย (เปลี่ยนชื่อ instance ได้ตรงนี้) และทุกเวอร์ชันทางขวา กรองได้ด้วย **All / Release / Beta / Alpha** แล้วกด **Install**

## นำเข้าไฟล์

เปิดแท็บ **Import** แล้วลากไฟล์ `.mrpack` หรือ `.zip` ของ CurseForge มาวาง หรือกดเพื่อเลือกไฟล์ ลันเชอร์จะแสดงเวอร์ชัน Minecraft, mod loader, จำนวนม็อดและไฟล์เสริมก่อนดาวน์โหลดอะไร เปลี่ยนชื่อได้ตามต้องการ แล้วกด **Import**

## การติดตั้งทำงานอย่างไร

- ติดตั้ง**แบบเบื้องหลัง** ปิดหน้าต่างได้ทุกเมื่อ instance ใหม่จะขึ้นที่แถบด้านข้างพร้อมวงแหวนความคืบหน้า
- กดที่ instance ที่กำลังติดตั้งเพื่อดูในหน้าแรก พร้อมแถบโหลดแบบเดียวกับตอนเปิดเกม หรือเปิดรายการ **Installs** (ปุ่มที่มีตัวเลขเหนือ **+**) เพื่อยกเลิก
- ทุกไฟล์ที่ดาวน์โหลดถูกตรวจสอบค่า hash ม็อดที่ผู้สร้างไม่อนุญาตให้ดาวน์โหลดผ่านโปรแกรมอื่นจะแสดงเป็นรายการพร้อมลิงก์ให้ดาวน์โหลดเอง
- ระบบเตรียม Minecraft และ mod loader (รวมขั้นตอน patch ของ NeoForge / Forge) ไว้ระหว่างติดตั้ง กด **Launch** ครั้งแรกเข้าเกมได้ทันที
- รีโหลดหน้าต่างลันเชอร์แล้วการติดตั้งยังทำงานต่อ

| แหล่ง | ต้องมี |
|---|---|
| Modrinth, FTB, ATLauncher | อินเทอร์เน็ต |
| CurseForge (ค้นหาหรือไฟล์ `.zip`) | อินเทอร์เน็ต **และ** Neko API ซึ่งใช้หาลิงก์ดาวน์โหลดของ CurseForge (คีย์ของ CurseForge อยู่บนเซิร์ฟเวอร์ของเรา) |
| Create | อินเทอร์เน็ตสำหรับดาวน์โหลด Minecraft และ loader ครั้งแรก |

---

## ตั้งค่า instance

คลิกขวาที่ instance ในเครื่อง (หรือเปิดเมนูข้างปุ่ม **Launch**) แล้วเลือก **Instance settings**

![เมนูคลิกขวาของ instance ในเครื่อง](https://neko-launcher.com/docs/local-instances/instance-menu.webp)

![หน้าตั้งค่า instance แท็บ Overview](https://neko-launcher.com/docs/local-instances/settings-overview.webp)

| หน้า | เปลี่ยนอะไรได้บ้าง |
|---|---|
| **Overview** | ชื่อ คำอธิบาย ไอคอน และรูปพื้นหลังในหน้าแรก แสดงที่มาของ modpack แบบอ่านอย่างเดียว |
| **Game** | เวอร์ชัน Minecraft และ mod loader (มีคำเตือนว่าอาจทำให้ม็อดใช้ไม่ได้) RAM ของ instance นี้ JVM arguments และ game arguments กดบันทึกที่แถบด้านล่าง |
| **Content** | ม็อด รีซอร์สแพ็ก เชดเดอร์แพ็ก และโลก |
| **Danger zone** | เปิดโฟลเดอร์ของ instance หรือลบ instance พร้อมไฟล์ทั้งหมด |

### Content

![ม็อดของ instance ในเครื่องพร้อมป้ายบอกที่มา](https://neko-launcher.com/docs/local-instances/settings-content.webp)

- ทุกไฟล์มีป้าย **Modrinth**, **CurseForge** หรือ **Local** (ไฟล์ที่ทั้งสองเว็บไม่รู้จัก) และแสดงชื่อกับไอคอนจริงของโปรเจกต์เมื่อหาเจอ
- สวิตช์ใช้**เปิด/ปิด**ม็อดหรือแพ็ก (เปลี่ยนชื่อไฟล์เป็น `.disabled` ไม่ได้ลบ)
- **Browse** เพิ่มคอนเทนต์จาก Modrinth และ CurseForge ส่วน **Add files** หรือการลากไฟล์มาวางใช้คัดลอกไฟล์ของคุณเอง
- **Check for updates** หาเวอร์ชันใหม่ของม็อดจาก Modrinth ที่เข้ากับเวอร์ชัน Minecraft และ loader นี้
- การลบไฟล์เป็นการลบถาวร (ไม่ไปที่ถังรีไซเคิล)

## ควรรู้

- Instance ในเครื่องยังอยู่ในแถบด้านข้างแม้เซิร์ฟเวอร์ Neko จะติดต่อไม่ได้ และมีป้าย **On this PC**
- Discord แสดงชื่อ instance ตอนเล่น แต่ไม่มี *Ask to Join* (ใช้กับเซิร์ฟเวอร์ Neko เท่านั้น)
- AI client ที่เชื่อมกับ MCP server ของลันเชอร์สร้าง นำเข้า ค้นหา และจัดการ instance ในเครื่องได้เช่นกัน ดู [MCP server](../neko-launcher/mcp-server.md)
