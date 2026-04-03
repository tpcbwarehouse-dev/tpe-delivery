# วิธี Deploy TPE ใบส่งมอบงาน

## ไฟล์ในโปรเจค
```
tpe-delivery/
├── index.html                          ← หน้าเว็บหลัก
├── netlify.toml                        ← config Netlify
├── package.json                        ← dependencies
├── netlify/functions/
│   └── delivery-proxy.js               ← API บันทึก/ดึงข้อมูล Google Sheet
└── DEPLOY.md                           ← ไฟล์นี้
```

---

## Step 1 — Share Google Sheet ให้ Service Account

เปิด Sheet: https://docs.google.com/spreadsheets/d/1QURgFIPLuVQvKSsVHa1_iT7juOKRnkCgz7oAX3pfKN0

1. คลิก **Share** (มุมขวาบน)
2. ใส่ email: `tpe-crm-sheets@tpe-crm-491307.iam.gserviceaccount.com`
3. เลือก **Editor**
4. คลิก **Send**

---

## Step 2 — สร้าง GitHub Repository

1. ไปที่ https://github.com/new
2. ตั้งชื่อ repo: `tpe-delivery`
3. เลือก **Public** (GitHub Pages ฟรีต้องเป็น Public)
4. คลิก **Create repository**

### Upload ไฟล์ขึ้น GitHub
```
วิธีที่ง่ายที่สุด: ลาก folder ทั้งหมดวางใน GitHub web UI
หรือใช้ git:

git init
git add .
git commit -m "initial deploy"
git remote add origin https://github.com/YOUR_USERNAME/tpe-delivery.git
git push -u origin main
```

---

## Step 3 — Deploy บน Netlify (รับ URL + ตั้ง Environment Variable)

1. ไปที่ https://netlify.com → Login
2. คลิก **Add new site** → **Import an existing project**
3. เลือก **GitHub** → เลือก repo `tpe-delivery`
4. Build settings ปล่อยว่างทั้งหมด (ไม่มี build command)
5. คลิก **Deploy site**

### ตั้ง Environment Variable
1. ไปที่ **Site configuration** → **Environment variables**
2. คลิก **Add a variable**
3. Key: `GOOGLE_SERVICE_ACCOUNT_KEY`
4. Value: วางเนื้อหาของไฟล์ `tpe-crm-491307-xxxx.json` (Service Account key ทั้งก้อน)
5. คลิก **Save**
6. **Trigger redeploy** (Deploys → Trigger deploy → Deploy site)

---

## Step 4 — อัพเดท API URL ในไฟล์ index.html

หลังจาก Netlify deploy เสร็จ จะได้ URL เช่น:
`https://amazing-name-12345.netlify.app`

เปิดไฟล์ `index.html` แก้บรรทัด:
```javascript
const API_URL = 'https://YOUR-NETLIFY-SITE.netlify.app/.netlify/functions/delivery-proxy';
```
เปลี่ยนเป็น URL จริงของคุณ แล้ว push ขึ้น GitHub อีกครั้ง Netlify จะ deploy ให้อัตโนมัติ

---

## ใช้งาน

เปิดเว็บ: `https://amazing-name-12345.netlify.app`

- **สร้างใหม่**: กรอกฟอร์ม → อัพรูป → กดสร้าง → ดาวน์โหลด .docx ทันที + บันทึกลง Sheet
- **ประวัติ**: แท็บ "ประวัติ" ดึงจาก Sheet → กด 👁 ดู / ⬇ โหลด .docx / 🖨 พิมพ์
- **Sheet**: ดูข้อมูลดิบได้ที่ Google Sheet โดยตรง

