# วิธีตั้งค่า Google Drive Sync (ทำครั้งเดียว)

ฟีเจอร์นี้ให้แต่ละท่าน **login ด้วย Google ของตัวเอง** เพื่อสำรอง/กู้คืนข้อมูลไปยัง Google Drive ส่วนตัว (ข้อมูลของแต่ละคนแยกกันโดยอัตโนมัติ ไม่ปนกัน และคนอื่นมองไม่เห็น)

ต้องตั้งค่า **Google Cloud Client ID** 1 ครั้ง แล้วใช้ได้กับทุกคนที่ใช้เว็บนี้ (ใส่ค่าเดียวในไฟล์ `config.js`)

---

## ขั้นตอนที่ 1: สร้างโปรเจกต์ใน Google Cloud

1. เปิด https://console.cloud.google.com/
2. ล็อกอินด้วย Google account (ใช้บัญชีไหนก็ได้ที่เป็นเจ้าของโปรเจกต์)
3. คลิกเมนูเลือกโปรเจกต์ด้านบน > **New Project**
4. ตั้งชื่อ เช่น `implant-tracker` แล้วกด **Create**

## ขั้นตอนที่ 2: เปิดใช้งาน Google Drive API

1. ในเมนูซ้าย ไปที่ **APIs & Services > Library**
2. ค้นหา "Google Drive API"
3. คลิกเข้าไป แล้วกด **Enable**

## ขั้นตอนที่ 3: ตั้งค่า OAuth consent screen

1. ไปที่ **APIs & Services > OAuth consent screen**
2. เลือก User type = **External** > Create
3. กรอกข้อมูลพื้นฐาน (App name: "บันทึกรากเทียม", User support email, Developer contact email)
4. หน้า Scopes: กด **Add or remove scopes** แล้วเลือก
   - `.../auth/drive.appdata`
   - `.../auth/userinfo.email`
5. หน้า Test users: เพิ่มอีเมล Google ของหมอทุกท่านที่จะใช้งาน (ขณะแอปยังเป็นสถานะ "Testing")
   - หากต้องการให้ใครก็สมัครใช้ได้โดยไม่ต้องเพิ่มทีละคน ให้กด **Publish App** (ต้องผ่านการตรวจสอบจาก Google สำหรับ scope ที่ sensitive)

## ขั้นตอนที่ 4: สร้าง OAuth Client ID

1. ไปที่ **APIs & Services > Credentials**
2. กด **Create Credentials > OAuth client ID**
3. Application type: **Web application**
4. Name: ตั้งชื่ออะไรก็ได้ เช่น `implant-tracker-web`
5. ที่ **Authorized JavaScript origins** ให้เพิ่ม URL ที่จะเปิดเว็บนี้ เช่น
   - `http://localhost:8000` (สำหรับทดสอบในเครื่อง)
   - ถ้าใช้ port อื่น ให้ใส่ port นั้นด้วย เช่น `http://localhost:5500`
   - ถ้าในอนาคต deploy ขึ้น GitHub Pages ให้เพิ่ม URL จริงด้วย เช่น `https://yourname.github.io`
6. กด **Create** จะได้ **Client ID** หน้าตาเช่น
   `123456789012-abcdefghijklmnop.apps.googleusercontent.com`

## ขั้นตอนที่ 5: ใส่ Client ID ในเว็บ

เปิดไฟล์ `config.js` แล้วแก้บรรทัด:

```js
const GOOGLE_CLIENT_ID = 'YOUR_GOOGLE_CLIENT_ID.apps.googleusercontent.com';
```

เปลี่ยนเป็น Client ID ของจริงที่ได้จากขั้นตอนที่ 4 แล้วบันทึกไฟล์

---

# วิธีรันเว็บผ่าน localhost (สำหรับทดสอบ)

Google sign-in **ใช้งานไม่ได้** กับการเปิดไฟล์ตรง ๆ (`file://...`) ต้องรันผ่านเว็บเซิร์ฟเวอร์เล็ก ๆ บนเครื่องตัวเอง

## วิธีที่ 1: ใช้ Python (มักมีอยู่ในเครื่องแล้ว)

1. เปิด Terminal
2. ไปที่โฟลเดอร์ของเว็บ:
   ```bash
   cd /Users/chitaphol/Claude/Projects/Implantologist/implant-tracker
   ```
3. รันคำสั่ง:
   ```bash
   python3 -m http.server 8000
   ```
4. เปิด Chrome ไปที่: **http://localhost:8000**

(กด Ctrl+C ใน Terminal เพื่อหยุดเซิร์ฟเวอร์)

## วิธีที่ 2: ใช้ VS Code

ติดตั้ง extension **Live Server** แล้วคลิกขวาที่ `index.html` > **Open with Live Server**

---

# วิธีใช้งานสำหรับหมอแต่ละท่าน

1. แชร์โฟลเดอร์ `implant-tracker` (ที่มี `config.js` ที่ตั้งค่าแล้ว) ให้หมอท่านอื่น หรือเมื่อ deploy เป็นเว็บไซต์ออนไลน์แล้วก็ส่งลิงก์เว็บให้
2. แต่ละท่านเปิดเว็บผ่าน Chrome แล้วเข้าไปที่แท็บ **ตั้งค่า**
3. กด **"เข้าสู่ระบบด้วย Google"** แล้วเลือกบัญชี Google ของตัวเอง
4. กด **"สำรองข้อมูลขึ้น Google Drive"** เพื่อ backup ข้อมูล หรือ **"กู้คืนข้อมูลจาก Google Drive"** เพื่อดึงข้อมูลที่เคย backup ไว้ (เช่น เปิดจากคอมพิวเตอร์/มือถือเครื่องอื่น)

ข้อมูลของแต่ละท่านจะถูกเก็บแยกกันในพื้นที่ส่วนตัว (Google Drive App Data) ของบัญชีตัวเอง คนอื่นไม่สามารถเข้าถึงข้อมูลของท่านได้ และข้อมูลจะไม่ปรากฏให้เห็นในหน้า Google Drive ปกติของท่าน (เป็นพื้นที่ซ่อนสำหรับแอป)

---

# หมายเหตุ

- ขณะนี้เว็บยังทำงานเป็น **local-first**: ข้อมูลหลักเก็บใน localStorage ของเบราว์เซอร์ในเครื่อง การ sync กับ Google Drive เป็นการสำรอง/กู้คืนแบบกดเอง (manual)
- หากยังไม่ตั้งค่า `config.js` เว็บจะยังใช้งานได้ตามปกติ เพียงแต่จะไม่แสดงปุ่มเชื่อมต่อ Google Drive
- การ deploy ขึ้นออนไลน์ (เช่น GitHub Pages) สามารถทำได้ในขั้นตอนต่อไป โดยต้องเพิ่ม URL ของเว็บไซต์จริงใน "Authorized JavaScript origins" ตามขั้นตอนที่ 4
