---
name: "phum-skill"
description: "Phumiphut's way of building software: talk → plan → spec → code with his approval at each stage, simple readable code, and his backend/frontend project structure. Use whenever writing code or starting a new project for him."
---

# phum-skill — วิธีทำงานและเขียนโค้ด

ใช้ได้ทุกโปรเจ็ค ไม่ผูกกับ stack ใด stack หนึ่ง

## 0. ก่อนเริ่ม

- ถ้าโปรเจ็คมี `CLAUDE.md` / README ให้อ่านก่อนเสมอ
- **ถ้ากฎของโปรเจ็คขัดกับ skill นี้ ให้ยึดของโปรเจ็ค**
- ก่อนสร้างของใหม่ หาในโปรเจ็คก่อนว่ามี pattern / helper / component เดิมไหม แล้วทำตามของเดิม
- ถามก่อนทำสิ่งที่ไม่ได้สั่งตรง ๆ เช่น สร้าง branch, ลบไฟล์, แก้โค้ดที่ผู้ใช้กำลังเขียนเอง

---

## 1. Flow: คุย → plan → spec → code

```
คุย ──► plan ──► spec ──► code
 ▲       │        │
 └───────┴────────┘
   ยังไม่ลงตัว = ย้อนกลับไปคุยใหม่
```

| ขั้น | ผลลัพธ์ | ไปขั้นต่อไปได้เมื่อ |
|---|---|---|
| **คุย** | เข้าใจตรงกันเรื่องเป้าหมาย ผู้ใช้ ขอบเขต ข้อจำกัด | ไม่มีคำถามค้าง |
| **plan** | ภาพรวม: ฟีเจอร์, stack, โครงโฟลเดอร์, ลำดับงาน | ผู้ใช้บอกว่า "plan ผ่าน" |
| **spec** | แต่ละฟีเจอร์ / ฟังก์ชันที่จำเป็น ทำอะไร | ผู้ใช้บอกว่า "spec ผ่าน" |
| **code** | เขียนตาม spec เท่านั้น | — |

**กติกา**
- **ข้ามขั้นไม่ได้** plan ยังไม่ผ่าน = ห้ามเขียน spec · spec ยังไม่ผ่าน = ห้ามเขียนโค้ด
- **"ผ่าน" ต้องมาจากผู้ใช้** Claude ตัดสินเองไม่ได้ว่าลงตัวแล้ว
- **ระหว่างเขียนโค้ดเจอสิ่งที่ spec ไม่ครอบคลุม** → หยุด กลับไปคุย/แก้ spec ก่อน ห้ามเดาแล้วเขียนต่อ
- **ย้อนกลับได้เสมอ** ไม่ถือว่าเสียเวลา ดีกว่าเขียนผิดทิศแล้วต้องรื้อ

### ขั้นคุย (โปรเจ็คใหม่)

ไม่รีบ ยังไม่สร้างไฟล์ ยังไม่เลือก stack จนกว่าจะเห็นภาพตรงกัน
ถามทีละเรื่อง ไม่ยิงคำถามรวดเดียว

- **เป้าหมาย** — ทำเพื่ออะไร (ส่งอาจารย์ / ฝึกเอง / ใช้งานจริง / portfolio)
- **ผู้ใช้** — ใครใช้ แต่ละกลุ่มทำอะไรได้บ้าง
- **ขอบเขต MVP** — อะไรต้องมีรอบแรก อะไรไว้ทีหลัง
- **ข้อจำกัด** — deadline, stack ที่ถูกกำหนด, ทำคนเดียวหรือเป็นทีม

เสร็จแล้วสรุปเป็น `plan.md` + `CLAUDE.md` ให้ผู้ใช้อ่านและยืนยัน

### ขั้น spec

**ทุกฟีเจอร์** — spec ระดับฟีเจอร์สั้น ๆ: ทำอะไร, ใครใช้, ผ่าน API / หน้าไหน

**ลงระดับฟังก์ชันเฉพาะตัวที่จำเป็น** (เข้าข้อใดข้อหนึ่ง):

| ประเภท | ตัวอย่าง |
|---|---|
| เกี่ยวกับเงิน | คำนวณราคา ส่วนลด ยอดรวม |
| สิทธิ์ / ความปลอดภัย | login, เช็ค ownership |
| ตรวจข้อมูลจากภายนอก | validate request |
| แก้ข้อมูลหลายที่พร้อมกัน | สร้างออเดอร์ + ตัดสต๊อก |
| มีเงื่อนไขแยกหลายทาง | การเปลี่ยนสถานะ |
| ถูกเรียกจากหลายที่ | helper ที่ใช้ร่วมกัน |

ไม่ต้องเขียน: ดึงข้อมูลตรง ๆ, CRUD ไม่มีเงื่อนไข, component ที่แค่แสดงผล
ไม่แน่ใจว่าจำเป็นไหม → ถามผู้ใช้

**รูปแบบ spec ระดับฟังก์ชัน**

```
### ชื่อฟังก์ชัน
ทำอะไร   ...
รับ      ...
คืน      กรณีสำเร็จ / กรณีผิดพลาด
กรณีผิด  อะไรบ้างที่ถือว่าผิด
ใครเรียก ...
ไม่ทำ    งานที่ไม่ใช่ของฟังก์ชันนี้ และใครเป็นคนทำแทน
```

ข้อ **ไม่ทำ** สำคัญ — บอกขอบเขตว่าฟังก์ชันนี้รับผิดชอบแค่ไหน

---

## 2. หลักการเขียนโค้ด: เข้าใจง่าย จำเป็นค่อยใส่

### เข้าใจง่าย ไม่ซับซ้อน
- อ่านจากบนลงล่างแล้วเข้าใจ ไม่ต้องกระโดดหลายไฟล์
- **`if` ซ้อนได้ไม่เกิน 3 ชั้น** ถ้าเกิน → เปลี่ยนเป็น guard clause (เช็คกรณีผิดแล้ว `return` ออกก่อน)
- ตั้งชื่อให้บอกว่าทำอะไร ไม่ย่อจนเดาไม่ออก
- ฟังก์ชันหนึ่งทำเรื่องเดียว
- เลือกแบบตรงไปตรงมา ไม่เขียนแบบฉลาดแต่อ่านยาก

```ts
// ✅ ซ้อน 2 ชั้น — อ่านเข้าใจ ไม่ต้องแก้
if (user) {
  if (user.role === "ADMIN") {
    showAdminMenu();
  }
}

// ❌ ซ้อนเกิน 3 ชั้น
if (order) {
  if (order.status === "PENDING") {
    if (user.id === order.user_id) {
      if (order.items.length > 0) {
        cancel(order);
      }
    }
  }
}

// ✅ เปลี่ยนเป็น guard clause
if (!order) return "ไม่พบคำสั่งซื้อ";
if (order.status !== "PENDING") return "ยกเลิกได้เฉพาะคำสั่งซื้อที่รอดำเนินการ";
if (user.id !== order.user_id) return "ไม่มีสิทธิ์";
if (order.items.length === 0) return "ไม่มีสินค้าในคำสั่งซื้อ";
cancel(order);
```

### จำเป็นค่อยใส่
- ไม่สร้าง abstraction / helper / config "เผื่อไว้" — ใช้ซ้ำจริง 2–3 ที่ค่อยแยก
- ไม่ลง library ถ้าเขียนเองไม่กี่บรรทัดได้
- คอมเมนต์อธิบาย **ทำไม** ไม่อธิบายว่าโค้ดทำอะไร
- **ข้อยกเว้น:** validation, error handling, ความปลอดภัย — ใส่เสมอ ถึงโค้ดจะยาวขึ้น

### Validate
- **validate ข้อมูลจากภายนอกที่ backend เสมอ** — frontend เช็คเพื่อ UX แต่ backend เป็นด่านที่ข้ามไม่ได้ (Postman, DevTools, script ส่งตรงได้หมด)
- TypeScript type หายตอน runtime — `req.body as X` ไม่ได้เช็คอะไร
- **วิธีเขียนตามที่โปรเจ็คใช้อยู่** (ฟังก์ชันเขียนเอง / Zod / Laravel validation ฯลฯ) ถ้ายังไม่มี → ถามก่อน

---

## 3. โครง backend

- **แบ่งโฟลเดอร์ตามเรื่อง** ไม่แบ่งตามชนิดไฟล์

```
modules/
├── brands/
│   ├── brands.routes.ts
│   └── brands.service.ts
└── products/
    ├── products.routes.ts
    └── products.service.ts
```

- **แยกชั้นให้ชัด แต่ละชั้นรู้แค่งานของตัวเอง**
  - **route** — คุยกับ HTTP: รับ request, validate, เลือก status code, ส่ง response
  - **service** — logic + คุยกับ DB ไม่รู้จัก `req` / `res` · หาไม่เจอคืน `null` ให้ route ตอบ 404
- **ค่าคงที่รวมไว้ที่ `config/`**
- **ไม่ใส่จนกว่าจะจำเป็น:** ชั้น repository แยกจาก service, dependency injection, interface ทุกเรื่อง

### Status code

| code | ใช้เมื่อ |
|---|---|
| 400 | ข้อมูลผิดรูปแบบ |
| 401 | ยังไม่รู้ว่าเป็นใคร (ไม่มี / token ผิด) |
| 403 | รู้แล้วแต่ไม่มีสิทธิ์ |
| 404 | ไม่เจอ |
| 409 | ชนกับของเดิม (ซ้ำ) |
| 500 | ฝั่งเราพัง |

รูปแบบ response (envelope) ให้ตามที่โปรเจ็คกำหนด ใช้แบบเดียวกันทั้งระบบ

---

## 4. โครง frontend (แม่แบบจาก tpso-rice-backoffice)

```
src/
├── config/app.ts            ค่าคงที่ทั้งระบบ แก้ที่นี่ที่เดียวตอนขึ้นโปรเจ็คใหม่ (APP_NAME, ROUTES, API URL, cookie keys)
├── types/                   type ที่ใช้ร่วมกัน
├── app/
│   ├── page.tsx             login
│   └── admin/
│       ├── layout.tsx       sidebar + navbar + content
│       └── <module>/        1 โมดูล = 4 หน้า
│           ├── page.tsx             list + filter + pagination
│           ├── create/page.tsx      ฟอร์มสร้าง
│           ├── edit/[id]/page.tsx   ฟอร์มแก้ไข + ลบ
│           └── detail/[id]/page.tsx หน้าอ่านอย่างเดียว
├── components/
│   ├── layout/              sidebar, navbar, tabs
│   └── common/              Loading, Toast, ConfirmModal, Pagination, Upload ...
├── service/
│   ├── http.ts              Get / Post / Update / Delete — เรียก API ที่นี่ที่เดียว
│   ├── permission.ts        เช็คสิทธิ์ (ถ้ามีระบบสิทธิ์)
│   └── common.tsx           helper format วันที่ / เบอร์โทร ฯลฯ
└── lib/                     ฟังก์ชันล้วน ไม่ผูก React
```

### ทำหน้าใหม่
- ก๊อปโมดูลแม่แบบที่มีอยู่ แล้วแก้ค่าคงที่ด้านบนไฟล์
  ```ts
  const ENDPOINT = "backoffice/news";   // path ของ API
  const MODULE_PATH = "/admin/news";    // path ของหน้า
  const PRIMARY_KEY = "news_id";        // ชื่อ id ของแถว
  ```
- เมนู active คำนวณจาก path — เพิ่มหน้าใหม่ไม่ต้องแก้ sidebar / navbar

### ลำดับในไฟล์หน้าเพจ

```
ค่าคงที่ (ENDPOINT, MODULE_PATH) → state → ฟังก์ชัน get… / on… → onInit → useEffect → loading → return JSX
```

### กติกาในแต่ละหน้า
- เริ่มด้วย `onInit()` ที่โหลดทุกอย่างพร้อมกันด้วย `Promise.allSettled`
  ```ts
  const onInit = async () => {
    try {
      await Promise.allSettled([getPermission(), getList("", "", 1, 10), getCategory()]);
    } finally {
      setIsLoading(false);
    }
  };
  useEffect(() => { onInit(); }, []);
  ```
- `getList()` **รับพารามิเตอร์ครบทุกตัว** ไม่อ่านจาก state ข้างใน (state ยังไม่อัปเดตตอนเรียก)
- ฟอร์มใช้ `formData` คู่กับ `errorFields` ที่ key ตรงกัน แล้ว `validateForm()` ตรวจครบทีเดียว
  ```ts
  const validateForm = () => {
    const errors: Record<string, string> = {};
    if (!formData.name) errors.name = "กรุณากรอกชื่อ";
    setErrorFields(errors);
    return Object.keys(errors).length === 0;
  };
  ```
- พิมพ์แก้ช่องไหน → ล้าง error ช่องนั้นทันที
- กด Save / Delete → เปิด **ConfirmModal** → กดยืนยันแล้วค่อยยิง API
- แจ้งผลด้วย **Toast** ไม่ใช้ `alert()` / `confirm()`
- ปุ่มบันทึกต้อง loading + disable ระหว่างยิง กันกดซ้ำ
- เรียก API ผ่าน `service/http.ts` เท่านั้น ห้าม `fetch` / `axios` ตรงในหน้าเพจ

### ถ้าโปรเจ็คมีระบบสิทธิ์
- โหลดสิทธิ์ของหน้าใน `onInit` พร้อมข้อมูล
- ปุ่มที่แก้ข้อมูลได้ ผูก `isDisabled` กับสิทธิ์ (create / update / delete)
- การซ่อนปุ่มเป็นแค่ UX — backend ต้องตรวจสิทธิ์ซ้ำเสมอ

### `"use client"` vs server component (Next.js)

| ชนิดหน้า | ทำแบบไหน |
|---|---|
| **หลังบ้าน / admin** (ต้อง login) | `"use client"` ทั้งหน้าได้เลย — ไม่ต้องทำ SEO หน้า interactive แทบทั้งหมด เขียนแบบเดียวกันทุกหน้า |
| **หน้าสาธารณะ** (หน้าร้าน / หน้าสินค้า) | server component เป็นหลัก แยกส่วนที่ต้องกดได้ออกเป็น component เล็กที่ใส่ `"use client"` |

---

## 5. ความปลอดภัยพื้นฐาน (ห้ามตัดทิ้ง)

1. SQL ใช้ parameterized (`$1, $2` / `?`) เสมอ ห้ามต่อ string เข้า SQL แม้ query จะ dynamic
2. role / user id มาจาก token ที่ verify แล้ว **ห้าม**รับจาก body
3. ตรวจสิทธิ์ที่ backend ทุก endpoint — ซ่อนปุ่มบน UI ไม่นับเป็นความปลอดภัย
4. ห้ามคืนข้อมูลลับใน response (password hash, token ของคนอื่น ฯลฯ)
5. ห้ามใส่ secret ในตัวแปรที่หลุดไปฝั่ง client (`NEXT_PUBLIC_*`, `VITE_*`)

---

## 6. ก่อนบอกว่าเสร็จ

- รัน type-check และ lint ให้ผ่าน
- เช็คบรรทัด import ที่ editor แอบเติมให้ (เช่น `import { error } from "console"`) — type-check มักไม่ฟ้องแต่โค้ดทำงานผิด
- type-check ผ่าน ≠ ทำงานถูก — logic ที่สำคัญต้องลองยิงจริง
- บอกผู้ใช้ตรง ๆ ว่าอะไรทดสอบแล้ว อะไรยังไม่ได้ทดสอบ