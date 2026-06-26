# 📖 คู่มือการใช้งาน Dental Implant Patient Record

---

## 1. ข้อมูลเก็บอยู่ที่ไหน?

ข้อมูลคนไข้ เคส และนัดหมาย **เก็บใน Browser ของเครื่องที่ใช้งาน** (localStorage) ซึ่งหมายความว่า:

- ✅ ใช้งานได้โดยไม่ต้องมีอินเทอร์เน็ต
- ⚠️ ถ้าเปลี่ยนเครื่อง หรือล้างแคช Browser → ข้อมูลหาย
- ⚠️ มือถือและ iPad คนละ copy กัน ไม่ sync อัตโนมัติ

**ภาพ X-ray** ถูกอัปโหลดขึ้น Google Drive โดยตรง (ต้องล็อกอิน Google)

---

## 2. เชื่อมต่อ Google Drive (ทำครั้งแรกครั้งเดียว)

> ใช้สำหรับ backup ข้อมูลคนไข้/เคส และเก็บภาพ X-ray

### ขั้นตอน:
1. เปิด app → แตะ **Settings** (ไอคอนหมู 🐷 ล่างขวา)
2. กดปุ่ม **"Sign in with Google"**
3. เลือกบัญชี Google ที่ต้องการใช้
4. อนุญาต permission ที่ขอ (เก็บไฟล์ใน Google Drive)
5. ด้านบนขวาจะแสดงชื่อ email ที่ล็อกอินแล้ว ✅

> **หมายเหตุ:** ถ้าปุ่ม Sign in ไม่ปรากฏ แสดงว่ายังไม่ได้ตั้งค่า Google Client ID  
> → อ่านขั้นตอนใน **`SETUP.md`** ในโฟลเดอร์แอป

---

## 3. สำรองข้อมูล (Backup)

### วิธีที่ 1: Backup ขึ้น Google Drive (แนะนำ)
1. ล็อกอิน Google ก่อน (ดูข้อ 2)
2. ไป **Settings → "Backup to Google Drive"**
3. กดปุ่ม → รอสักครู่ → ขึ้น toast "Backed up to Google Drive" ✅
4. ไฟล์จะถูกเก็บใน Google Drive ของคุณโดยอัตโนมัติ

### วิธีที่ 2: Export ไฟล์ .json ลงเครื่อง
1. ไป **Settings → "Backup Data (.json)"**
2. ไฟล์จะดาวน์โหลดลงเครื่อง → เก็บไว้ที่ปลอดภัย
3. ใช้ไฟล์นี้ import กลับได้ทุกเมื่อ

### วิธีที่ 3: Export Excel (CSV)
- ใช้สำหรับเปิดดูใน Excel หรือส่งต่อ
- ไป **Settings → "Export as Excel (CSV)"**

---

## 4. Sync ข้อมูลระหว่างเครื่อง

> เช่น backup จากคอมพิวเตอร์ → กู้คืนในมือถือ

### ขั้นตอน:

**เครื่องที่ 1 (ต้นทาง):**
1. ล็อกอิน Google
2. Settings → **"Backup to Google Drive"**

**เครื่องที่ 2 (ปลายทาง — มือถือ/iPad):**
1. เปิด app บนเครื่องใหม่
2. ล็อกอิน **Google บัญชีเดียวกัน**
3. Settings → **"Restore from Google Drive"**
4. ยืนยัน → ข้อมูลจะถูก sync มาให้

> ⚠️ การ Restore จะ **แทนที่ข้อมูลเดิม** ในเครื่องนั้นทั้งหมด

---

## 5. เปิดในมือถือ (iPhone / Android)

### กรณี A: แอปอยู่บน Internet (Netlify / GitHub Pages)

1. เปิด **Safari (iPhone)** หรือ **Chrome (Android)**
2. พิมพ์ URL ของแอป เช่น `https://your-app.netlify.app`
3. **เพิ่มลง Home Screen** เพื่อใช้แบบ App:
   - **iPhone (Safari):** กดไอคอน Share → "Add to Home Screen" → Add
   - **Android (Chrome):** กด ⋮ menu → "Add to Home Screen" → Add
4. แอปจะปรากฏบน Home Screen เหมือน App จริง ใช้ offline ได้ด้วย ✅

### กรณี B: รันบนเครื่องเดียวกัน (localhost)

ต้องเปิดให้มือถือในวง WiFi เดียวกันเข้าถึงได้:

1. รัน server บนคอม: `python3 -m http.server 8080` ในโฟลเดอร์แอป
2. หา IP เครื่องคอม เช่น `192.168.1.10`
3. เปิดมือถือ → Browser → พิมพ์ `http://192.168.1.10:8080`
4. ใช้งานได้ (แต่ไม่มี Add to Home Screen สำหรับ localhost)

---

## 6. ภาพ X-ray

- X-ray **ผูกกับรากเทียมแต่ละซี่** (ไม่ได้เก็บในเครื่อง)
- อัปโหลดขึ้น **Google Drive** โดยตรงในโฟลเดอร์ "Implant Tracker X-rays"
- ต้องล็อกอิน Google ก่อนถึงจะอัปโหลด/ดูภาพได้
- ถ้าเปลี่ยนเครื่อง → ล็อกอิน Google บัญชีเดิม → ภาพยังอยู่ครบ ✅

---

## สรุปภาพรวม

| สิ่งที่เก็บ | เก็บที่ไหน | sync อัตโนมัติ? |
|---|---|---|
| ข้อมูลคนไข้ / เคส / นัด | Browser (localStorage) | ❌ ต้อง Backup/Restore เอง |
| ภาพ X-ray | Google Drive | ✅ ทุก account เดียวกัน |
| Backup (.json) | Google Drive (appData) | ❌ กด Backup เอง |

### 💡 แนะนำ: Backup ทุกครั้งก่อนปิดแอป
Settings → "Backup to Google Drive" → ใช้เวลาไม่ถึง 3 วินาที

---

*Dental Implant Patient Record · ข้อมูลอัปเดตล่าสุด 2025*
