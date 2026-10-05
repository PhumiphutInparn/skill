# phum-skill

Claude skill ที่เก็บวิธีทำงานและสไตล์การเขียนโค้ดของผม — ใช้ได้ทุกโปรเจ็ค ไม่ผูกกับ stack

- **Flow:** คุย → plan → spec → code ย้อนกลับได้ ข้ามขั้นไม่ได้ ทุกขั้นต้องได้ "ผ่าน" ก่อน
- **Spec:** ทุกฟีเจอร์ + ระดับฟังก์ชันเฉพาะตัวที่จำเป็น (มีช่อง "ไม่ทำ")
- **โค้ด:** เข้าใจง่าย จำเป็นค่อยใส่ · `if` ซ้อนไม่เกิน 3 ชั้น
- **โครงโปรเจ็ค:** backend แบ่งตามเรื่อง แยก route / service · frontend ตามแม่แบบ backoffice
- **ความปลอดภัยพื้นฐาน** และ **เช็คก่อนบอกว่าเสร็จ**

รายละเอียดทั้งหมดอยู่ใน [`SKILL.md`](./SKILL.md)

## ติดตั้ง

**Claude Code**

```bash
mkdir -p ~/.claude/skills/phum-skill
cp SKILL.md ~/.claude/skills/phum-skill/
```

**Claude (claude.ai / desktop)** — อัปโหลด `SKILL.md` ที่หน้า Settings → Skills
