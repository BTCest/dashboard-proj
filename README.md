# Dashboard หน้าเดียว — deploy ฟรี

ไฟล์ทั้งหมด: `index.html` + `data.json` เท่านั้น ไม่ต้อง build ไม่ต้องลง Node

## ทดสอบบนเครื่อง

ห้าม double-click ไฟล์ `index.html` ตรงๆ เพราะ `fetch()` จะถูกบล็อกด้วย CORS
ให้เปิดผ่านเว็บเซิร์ฟเวอร์แทน:

```bash
cd โฟลเดอร์นี้
python3 -m http.server 8000
# เปิด http://localhost:8000
```

## Deploy: Cloudflare Pages (แนะนำ)

1. สร้าง repo ใหม่บน GitHub แล้ว push ไฟล์ทั้งสองขึ้นไป
2. ไปที่ dash.cloudflare.com → Workers & Pages → Create → Pages → Connect to Git
3. เลือก repo
4. **Build command:** เว้นว่าง **Output directory:** `/` (หรือ `.`)
5. Save and Deploy

ได้ URL `ชื่อโปรเจกต์.pages.dev` ภายใน ~1 นาที
หลังจากนี้ทุกครั้งที่ `git push` เว็บจะ deploy ใหม่อัตโนมัติ

Free tier: bandwidth ไม่จำกัด, build 500 ครั้ง/เดือน, custom domain ฟรี (ผูกโดเมนตัวเองได้ ถ้ามี)

## ทางเลือก: GitHub Pages

Repo → Settings → Pages → Source: `Deploy from a branch` → `main` / `root` → Save
ได้ URL `username.github.io/ชื่อ-repo` (ต้องเป็น repo public ถ้าใช้บัญชีฟรี)

## ต่อโดเมนของตัวเอง

โดเมน `.com` ประมาณ 350–500 บาท/ปี (Cloudflare Registrar ขายราคาทุน)
ถ้าไม่อยากเสียเงินเลย ใช้ `*.pages.dev` ต่อไปได้ไม่มีวันหมดอายุ

## แก้ข้อมูล

แก้แค่ `data.json` อย่างเดียว — โครงสร้าง:

- `meta` — ชื่อหัวเรื่อง, คำบรรยาย, หน่วย
- `kpis[]` — การ์ดตัวเลขด้านบน (`format` ใช้ได้ `currency` หรือ `number`, `spark` คือจุดกราฟจิ๋ว)
- `monthly` — กราฟเส้น (ใส่กี่ series ก็ได้)
- `channels[]` — กราฟแท่งแนวนอน
- `table` — ตารางล่างสุด (คอลัมน์สุดท้ายจะเรนเดอร์เป็น % เติบโต)

## ถ้าอยากให้ข้อมูลอัปเดตเองโดยไม่ต้อง push

ทำ GitHub Actions ตั้งเวลา (cron) ให้ดึงข้อมูลจากแหล่งอื่นมาเขียนทับ `data.json`
แล้ว commit อัตโนมัติ — ยังฟรีอยู่ (2,000 นาที/เดือน)
