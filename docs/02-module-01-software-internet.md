# Module 1 — Software, Internet & APIs สำหรับคนไม่เคยเขียนโปรแกรม

⏱️ **20 ชั่วโมง (สัปดาห์ 1–2)** · 🎯 ผลลัพธ์: REST client ขนาดเล็ก + repo ที่มีประวัติ commit จริง

> **หลักคิด:** คุณไม่ต้องสร้างเว็บทั้งเว็บ แต่คุณ **ต้อง** เข้าใจว่าเว็บทำงานอย่างไร
> เพราะงาน FDE 80% คือการต่อระบบหนึ่งเข้ากับอีกระบบหนึ่ง

---

## 1.1 Software ประกอบด้วยอะไร

ทุกโปรแกรมคือ 4 ชั้นนี้:

```
┌─────────────────────────────┐
│  UI / Frontend              │  ← สิ่งที่คนเห็น (React, HTML, mobile app)
├─────────────────────────────┤
│  Business logic / Backend   │  ← กฎการทำงาน (Python/Node/Java)
├─────────────────────────────┤
│  Data                       │  ← เก็บของ (Postgres, S3, vector DB, warehouse)
├─────────────────────────────┤
│  Infrastructure             │  ← ที่อยู่ (AWS/Azure, Docker, Kubernetes)
└─────────────────────────────┘
```

**Lab 1.1 (1 ชม.):** เลือกแอปที่คุณใช้ทุกวัน (เช่น LINE, Grab) แล้วเขียนตอบทีละชั้นว่าแต่ละชั้นคืออะไร
และถ้าแต่ละชั้นล่มจะเกิดอะไรขึ้น → เก็บไว้ใน repo ที่ `notes/module-01/layers.md`

---

## 1.2 เกิดอะไรขึ้นเมื่อเปิดเว็บหนึ่งหน้า

ลำดับเหตุการณ์จริง (ต้องเข้าใจทั้งหมด — เป็นคำถาม interview ยอดนิยม):

1. **DNS lookup** — แปลง `example.com` → IP address (เหมือนสมุดโทรศัพท์)
2. **TCP/TLS handshake** — เปิดการเชื่อมต่อ + เข้ารหัส
3. **HTTP request** — ส่ง `GET /` พร้อม header (เช่น `Authorization`)
4. **Server ตอบ** — ส่ง status code + HTML/JSON
5. **Browser render** — แปลง HTML เป็นหน้าที่เห็น
6. **JS ทำงาน** — บางหน้าเรียก API เพิ่ม (นี่คือจุดที่มีปัญหาเวลา integrate)

**Lab 1.2 (2 ชม.):** เปิด terminal แล้วรัน
```bash
curl -v https://api.github.com/users/octocat
```
ไล่อ่าน output ทีละบรรทัด แล้วเขียนอธิบายว่าแต่ละส่วนคืออะไร
- `> GET /users/octocat HTTP/2`
- `< HTTP/2 200`
- `< content-type: application/json`
- body เป็น JSON

---

## 1.3 HTTP: สิ่งที่ FDE ใช้ทุกวัน

### Methods
| Method | ใช้ทำอะไร | ตัวอย่าง |
|---|---|---|
| `GET` | ขอข้อมูล (ต้องไม่เปลี่ยนข้อมูล) | ดูรายการคำสั่งซื้อ |
| `POST` | สร้างของใหม่ | สร้าง ticket |
| `PUT` / `PATCH` | แก้ของที่มีอยู่ (ทั้งก้อน / บางส่วน) | อัปเดตสถานะ |
| `DELETE` | ลบ | ลบเอกสาร |

### Status codes ที่ต้องรู้ทันที
| Code | ความหมาย | FDE เห็นแล้วต้องทำอะไร |
|---|---|---|
| `200` `201` | สำเร็จ / สร้างสำเร็จ | – |
| `400` | request ผิดรูป | ตรวจ payload, schema |
| `401` | ไม่ได้ authenticate | ตรวจ API key / token หมดอายุ |
| `403` | authenticate แล้วแต่ไม่มีสิทธิ์ | ตรวจ permission scope |
| `404` | ไม่เจอ | ตรวจ path, ID |
| `429` | ยิงถี่เกิน | ต้องมี rate limiting + retry with backoff |
| `500` `502` `503` | server ฝั่งโน้นพัง | retry + แจ้ง + มี fallback |

> 💡 **429 คือเพื่อนร้ายของ FDE** — ระบบ LLM ทุกเจ้าเจอ ตอน Module 4 จะกลับมาเจออีก

### Headers ที่ต้องเข้าใจ
- `Authorization: Bearer <token>` — การพิสูจน์ตัวตน
- `Content-Type: application/json` — บอกว่า body เป็นอะไร
- `Idempotency-Key` — กันการ POST ซ้ำ (สำคัญมากเวลารetry)

---

## 1.4 API: ภาษากลางระหว่างระบบ

**REST** = สไตล์ API ที่ใช้ HTTP, ทุกอย่างเป็น "resource" ที่มี URL

```
GET    /api/v1/customers/42
GET    /api/v1/customers/42/orders?status=open&limit=20
POST   /api/v1/tickets
PATCH  /api/v1/tickets/1001
```

กฎที่ควรจำ:
- **Query params** (`?key=value`) สำหรับกรอง/แบ่งหน้า
- **Path params** (`/customers/42`) สำหรับระบุตัวตน
- **Body** (JSON) สำหรับข้อมูลที่ส่งไป
- **Pagination** มีเกือบทุก API จริง — ถ้าไม่จัดการ คุณจะได้ข้อมูลแค่ 100 แถวแรก

### Types ของ API ที่เจอบ่อย
| ประเภท | ลักษณะ | ข้อควรระวัง |
|---|---|---|
| REST | JSON over HTTP | pagination, rate limit |
| GraphQL | query เลือก field ได้ | complexity, N+1 |
| SOAP | XML, เก่า | องค์กรใหญ่/รัฐยังใช้ |
| Webhook | ฝั่งโน้นยิงมาหาเรา | ต้อง verify signature, retry, idempotent |
| Batch/file | SFTP, S3 drop | นี่คือ "integration จริง" ของ enterprise จำนวนมาก |

**Lab 1.3 (4 ชม.):** สร้าง "API client" ด้วย `curl` + `jq` อย่างน้อย 5 คำสั่ง ต่อ API จริง 1 ตัว
(แนะนำ: GitHub API, Open-Meteo, หรือ public API ของไทย)
```bash
curl -s "https://api.github.com/repos/python/cpython/issues?state=all&per_page=100&page=2" | jq '.[0] | {title, state, user: .user.login}'
```
เก็บคำสั่งทั้งหมดใน `scripts/api-practice.sh` พร้อม comment อธิบาย

---

## 1.5 Git — เครื่องมือที่ขาดไม่ได้

**ทำไม FDE ต้องใช้ Git:** คุณจะแก้โค้ดใน repo ของลูกค้า ทำงานร่วมกับทีมเขา
และ**ประวัติ commit คือหลักฐานการส่งมอบของคุณ**

### คำสั่งที่ใช้จริง 90%
```bash
git clone <url>                  # ดึง repo
git switch -c feature/my-work    # สร้าง branch ใหม่
git status                       # ดูสถานะ (ใช้บ่อยที่สุด)
git add -p                       # เลือกทีละ hunk ที่จะ commit (ทักษะสำคัญ!)
git commit -m "feat: add retry with backoff"
git push origin feature/my-work
git pull --rebase                # ดึงของใหม่แบบสะอาด
git log --oneline --graph -20    # ดูประวัติ
git diff main...HEAD             # ดูว่าต่างจาก main อะไร
```

### Conventional Commits (มาตรฐานที่ทำให้ดูมืออาชีพ)
```
feat: เพิ่มฟีเจอร์
fix: แก้บั๊ก
docs: เอกสาร
refactor: ปรับโค้ดโดยไม่เปลี่ยนพฤติกรรม
test: เพิ่ม test
chore: งานจิปาฐะ
```

**Lab 1.4 (4 ชม.):**
1. สร้าง repo `fde-journey` (private ก็ได้)
2. ทำ branch, commit อย่างน้อย 10 commit, เปิด PR ของตัวเอง แล้ว merge
3. **จงใจสร้าง merge conflict** แล้วแก้ด้วยตัวเอง
4. เขียน `notes/module-01/git-cheatsheet.md` ด้วยภาษาของตัวเอง (ห้ามก๊อป)

---

## 1.6 Cloud & สภาพแวดล้อม

| บริการ | ใช้ทำอะไร | ทำไม FDE ต้องรู้ |
|---|---|---|
| Compute (EC2/VM/Cloud Run) | รันโค้ด | ที่ที่ service ของคุณจะอยู่ |
| Object storage (S3/GCS) | เก็บไฟล์/เอกสาร | ที่เก็บเอกสารสำหรับ RAG |
| Managed DB (RDS/Cloud SQL) | ฐานข้อมูล | ที่เก็บข้อมูลแอป |
| Secrets manager | เก็บ key/password | **ห้าม hardcode key ในโค้ด** |
| IAM / roles | สิทธิ์ | ต้นเหตุของ error 403 ที่พบบ่อยที่สุด |
| Region | ที่ตั้ง data center | ผูกกับกฎหมาย data residency (Module 9) |

**Lab 1.5 (2 ชม.):** อ่าน pricing page ของผู้ให้บริการ 1 ราย แล้วตอบ:
- ถ้ารัน service เล็ก ๆ ตลอดเดือน + เก็บไฟล์ 100 GB + LLM call 50,000 ครั้ง/เดือน ≈ เท่าไร
- ต้นทุนส่วนไหนที่ "โตตามการใช้งาน" และส่วนไหน "คงที่"

> ทักษะการประเมินต้นทุนนี้จะใช้ตรง ๆ ใน Module 12 (ROI)

---

## 1.7 Docker — ทำไม "มันรันในเครื่องผมได้" ไม่พอ

ปัญหา: โค้ดรันในเครื่องคุณ แต่พอขึ้นเครื่องลูกค้าพัง เพราะ Python version ต่าง / library ขาด

Docker แก้ด้วยการ**ห่อโค้ด + สภาพแวดล้อมทั้งหมด** เป็น image เดียว

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["python", "main.py"]
```

```bash
docker build -t my-app .
docker run --rm -e API_KEY=xxx my-app
```

**Lab 1.6 (2 ชม.):** ห่อสคริปต์จาก Lab 1.3 เป็น Docker image แล้วรันได้

---

## 📝 แบบฝึกหัดท้าย Module (ส่งงาน)

1. **REST client script** — bash หรือ Python ที่เรียก API จริง 3 endpoint, จัดการ pagination, มี error handling
2. **`notes/module-01/`** — 4 ไฟล์: layers, http-walkthrough, git-cheatsheet, cloud-cost
3. **repo `fde-journey`** — มี ≥10 commit, ≥1 PR ที่ merge แล้ว, README อธิบายว่าคืออะไร

## ✅ เกณฑ์ผ่าน

- [ ] อธิบายเส้นทาง request–response ได้โดยไม่ดูโน้ต
- [ ] เรียก API ด้วย curl/Python ได้เอง รวมถึง debug 401/403/429
- [ ] ใช้ Git branch + PR + แก้ conflict ได้
- [ ] เข้าใจความต่างระหว่าง compute/storage/DB/secrets/region
- [ ] รัน Docker container ของตัวเองได้
- [ ] ทำ Lab ครบ 6 ข้อ

**ถ้าติดข้อไหนนานเกิน 1 สัปดาห์** → ถามใน community หรือใช้ AI อธิบาย แล้ว**เขียนสรุปด้วยตัวเอง**
(อย่าปล่อยให้ค้าง — แต่ก็อย่าโกงตัวเองด้วยการข้าม)

---

**ต่อไป →** [`03-module-02-data-sql.md`](03-module-02-data-sql.md)
