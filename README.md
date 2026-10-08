# ทางเข้าระบบลาออนไลน์ของโรงเรียน

หน้า GitHub Pages เป็นทางเข้าไปยัง LIFF ที่ให้บริการโดย Google Apps Script ไม่มีการส่งคำขอหรือข้อมูลบุคลากรจาก GitHub Pages ไปยัง JSON API เดิม

## ก่อน merge และเปิดใช้งาน

ปัจจุบันที่ตรวจเมื่อ 8 ตุลาคม 2569 เว็บแอปยังใช้งาน Apps Script version 9 หน้า portal นี้ต้องเปิดใช้หลังย้ายเป็น v3 เท่านั้น

1. สำรอง Spreadsheet และไฟล์ประกอบ ตรวจการย้ายระบบและบัญชีผู้ดูแลใน staging ตาม README ของชุด v3
2. Deploy Apps Script v3 โดยแก้ deployment เดิมให้เป็นเวอร์ชันใหม่เพื่อคง URL เดิม หากออก deployment ใหม่ให้แก้ href ใน index.html เป็น URL ใหม่ด้วย
3. ตั้งค่า LINE Login Channel ID และ LIFF ID ในระบบ ตรวจการเชื่อมบัญชีและขั้นอนุมัติ
4. ใน LINE Developers Console เปลี่ยน LIFF Endpoint URL จาก GitHub Pages เป็น:

   `https://script.google.com/macros/s/AKfycbw-Dz-7LKc0fMXIMZXzC7RyHmlGAbofVDOVf4c5Jbn0aYNNmN4av9HomtJdna5Yd-lBEA/exec?page=liff`

5. ใช้ลิงก์ `https://liff.line.me/{LIFF_ID}` ใน Rich Menu และข้อความแจ้งเตือน ตรวจว่าลิงก์นี้เปิด Apps Script แทนกลับมาหน้า GitHub
6. ทดสอบ LINE บน iOS/Android, การเปิดจาก browser, รหัสผูกบัญชี, ยื่นคำขอ, เปิดไฟล์ private, การแก้ไข/ยกเลิก และผู้อนุมัติเปิดข้อความเก่า
7. Merge Pull Request นี้หลังปลายทางพร้อม GitHub Pages จะเป็นหน้าปุ่มทางเข้าสำหรับผู้ที่ยังใช้ bookmark เก่า

ห้ามตั้ง LIFF Endpoint เป็นหน้า portal หลังย้าย เพราะอาจทำให้วนกลับมาหน้าทางเข้าระหว่าง login และห้ามนำ liff.html ที่มี google.script.run มาเผยแพร่ตรงบน GitHub Pages

## พฤติกรรมและ rollback

- ไม่มีการ redirect อัตโนมัติ ผู้ใช้เห็นปลายทางก่อนกดปุ่ม ใช้งานได้แม้ปิด JavaScript
- ส่งต่อเฉพาะ `request` ที่ผ่านรูปแบบ และ `mode=approval` ไม่ส่งต่อ access token, LINE user ID หรือ URL ปลายทางจาก query
- หน้าเว็บไม่มี trackers, external scripts, API secrets หรือฟอร์มข้อมูลส่วนบุคคล
- หากต้องย้อน ให้ย้อน commit นี้และคืน LIFF Endpoint ให้สอดคล้องกับ backend/deployment รุ่นที่กู้กลับ ห้ามให้ frontend รุ่นเก่าเรียก backend v3

## ตรวจสอบ

ทดสอบ HTML ด้วย jsdom: ลิงก์ปกติ, request/mode ที่อนุญาต, ไม่ส่งต่อ token/URL แปลกปลอม และปุ่มใช้งานโดยไม่มี JavaScript ต้องทดสอบ Apps Script และ LINE จริงก่อน merge ตามรายการด้านบน
