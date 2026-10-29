# PROGRESS

- **วันปัจจุบัน:** Day 02
- **เส้นทางที่เลือก:** 4 สัปดาห์ (ครบทุกวัน + วันสำรองแก้ REVISION)
- **เริ่มเรียน:** 2026-10-26
- **เป้าส่งใบสมัคร:** —

## Setup

| รายการ | ค่า |
|---|---|
| Dataset | KolektorSDD2 |
| เครื่องเทรน | NVIDIA GeForce RTX 3060 Laptop GPU, 6 GB VRAM (local) |
| Python / PyTorch / CUDA | Python 3.14.7 / torch 2.13.0+cu130, torchvision 0.28.0+cu130 / CUDA 13.0 — `cuda.is_available()` = True (2026-10-28) |
| GitHub repo | https://github.com/SabigBenmumin/defect-inspect |

## Session log (ใหม่สุดอยู่บน)

- 2026-10-29 — Day 01 review รอบ 4: **PASS** — ตารางคำถามลูกค้า 6 ข้อ, ทิศทาง FA/FR ต่อชิ้นถูก, 6d ครบ; สรุปทั้ง 4 รอบอยู่ใน `reviews/day01-review.md` → เลื่อนเป็น Day 02
- 2026-10-29 — Day 01 review รอบ 3: REVISION (เหลือ 3 จุดเล็ก: ตารางคำถามลูกค้ายังไม่ทำ, ทิศทาง FA ต่อชิ้นผิด, 6d เงื่อนไข FA + 890) — Req-3, margin, assumption แก้แล้ว
- 2026-10-29 — Day 01 review รอบ 2: REVISION (เหลือ 5 จุด: ตารางคำถามลูกค้า, Req-3 p50, margin บรรทัด 68, assumption หน่วยตัดสิน, 6d) — 6b/ไฟล์/intro/.gitignore แก้แล้ว
- 2026-10-29 — เลือกเส้นทาง 4 สัปดาห์; ปิด session ค้างแก้ problem_spec ตาม review
- 2026-10-29 — Day 01 review: REVISION (problem_spec ขาด Req-3 detail, คำถามลูกค้าไม่มีผลต่อการออกแบบ, 6d overclaim, margin 20% ผิด, positive class ใน intro, ไฟล์ผิดที่)
- 2026-10-28 — Day 01 explain-back 5/6 PASS (rule of three ได้ 1); note อยู่ใน Obsidian (path ใน `notes/day01-notepath.md`)

> หมายเหตุ 2026-10-29: แก้วันที่ของ review รอบ 2–3 จาก 10-30 เป็น 10-29 (บันทึกวันที่ผิดไว้ก่อนหน้า)

## สรุปรายวัน

| Day | หัวข้อ | Lecture | Explain-back | Review | หมายเหตุ |
|---|---|---|---|---|---|
| 01 | Industrial inspection และ FR/FA | ✅ สร้างแล้ว | ✅ 5/6 | ✅ PASS รอบ 4 (2026-10-29) | env ✅, git ✅, problem_spec ✅ — ยังไม่ commit `.gitignore` + `problem_spec.md`; สรุป: `reviews/day01-review.md` |
| 02 | ภาพในฐานะข้อมูล และ CNN | — | — | — | |

## คะแนน

| การประเมิน | คะแนน | ผ่าน | วันที่ |
|---|---|---|---|
| Explain-back Day 01 | 5/6 | ✅ | 2026-10-28 |
| Review Day 01 | PASS (รอบ 4) | ✅ | 2026-10-29 |

## Results log (แหล่งเดียวของตัวเลขใน README และ resume)

| Run ID | วันที่ | Model / config | Split | Metric | ค่า | หมายเหตุ |
|---|---|---|---|---|---|---|

## Decisions log

| วันที่ | การตัดสินใจ | เหตุผล |
|---|---|---|
| 2026-10-29 | ประเมินบน KSDD2 แบบ 1 ภาพ = 1 ชิ้น | dataset ไม่มี piece ID; ถ้าไลน์จริงหลายภาพต่อชิ้นต้องคำนวณ FR/FA ใหม่ |
| 2026-10-29 | เส้นทาง 4 สัปดาห์ | Sabig เลือก: ครบทุกวันและมีวันสำรองแก้ REVISION |
| 2026-10-25 | ใช้ KolektorSDD2 | มีภาพ OK และ NG (วัด FR ได้), imbalance แบบไลน์ผลิต, มี pixel mask |

## จุดอ่อน

| หัวข้อ | มาจาก | วันที่ | สถานะ |
|---|---|---|---|
| ที่มาของ rule of three: (1−p)^n = 0.05 และ ln(1−p) ≈ −p | Explain-back Day 01 ข้อ 3 | 2026-10-28 | ✅ ปิด — 2026-10-29 เขียน derivation ครบใน problem_spec 6b |
| ใช้ p50 กับ deadline ของไลน์ (ควรเป็น tail percentile) | Review Day 01 รอบ 2 | 2026-10-29 | 🟡 ดีขึ้น — แยกกรณีสายพานหยุดรอ/เดินต่อเนื่องแล้ว (p99 ถูก) |
| FR ยืนยันได้แม้มี error > 0 (894 ภาพดี: FR ได้ถึง 33 ภาพ ≈ 3.7% แล้วขอบบนยัง ≤ 5%) | Review Day 01 รอบ 2 | 2026-10-29 | 🟡 เขียนใน spec 6d แล้ว — ทวนใน quiz 1 |
| FA = 0% พิสูจน์ไม่ได้ด้วย n จำกัด (เคยตอบว่า 3,000 ภาพพอ ซึ่งจริงคือ 0.1%) | Explain-back Day 01 ข้อ 3 | 2026-10-28 | เปิด — ทวนใน quiz 1 |
| นิยาม positive class ด้วย actual ไม่ใช่ prediction | Explain-back Day 01 ข้อ 1 | 2026-10-28 | ✅ ปิด — 2026-10-29 Sabig อธิบายได้ว่าบรรทัด 4 หมายถึง predicted positive |
| "margin" ของขนาด test ≠ การรองรับความผิดพลาด: หลุด 1 ต้อง n ≈ 947 ไม่ใช่ 720 | Review Day 01 | 2026-10-29 | ✅ ปิด — 2026-10-29 ลบออกจาก spec แล้ว |
| หลายภาพต่อชิ้น + กฎ OR: FR ต่อชิ้นขึ้น แต่ FA ต่อชิ้นลง | Review Day 01 รอบ 3 | 2026-10-29 | 🟡 spec แก้ทิศทางถูกแล้ว (รอบ 4) — ยังไม่ได้ส่งตัวเลขที่คำนวณ (3 ภาพ/ชิ้น) |
| Overclaim: สรุปว่าโมเดลผ่าน spec ทั้งที่ยังไม่มีโมเดล / ยังไม่ได้วัด | Review Day 01 | 2026-10-29 | ✅ ปิด — 2026-10-29 6d เขียนเป็นเงื่อนไข "ถ้า…" ครบแล้ว |

## Hint log

| Day | งาน | ขั้น hint สูงสุด | วันที่ |
|---|---|---|---|

## Glossary

| ศัพท์ | ความหมายสั้น ๆ |
|---|---|
| FR (False Reject, overkill) | ชิ้นดีถูกตีกลับ = FP / (FP + TN) ตัวหาร = ชิ้นดีทั้งหมด |
| FA (False Accept, escape) | ชิ้นเสียหลุด = FN / (FN + TP) = 1 − recall ตัวหาร = ชิ้นเสียทั้งหมด |
| Operating point | คู่ (FR, FA) ที่ threshold ที่เลือกใช้ |
| Constraint-first | ล็อก FA ตาม spec แล้วเลือก threshold ที่ FR ต่ำสุด บน val |
| Rule of three | 0 ความผิดพลาดใน n ครั้ง → ขอบบน 95% ≈ 3/n |
| p50 / p95 latency | percentile ของเวลาตอบสนอง: 50% ของชิ้นเร็วกว่า p50, 95% เร็วกว่า p95 (spec ไลน์ผลิตมักใช้ p95/p99) |
| Gray zone | ช่วง score ระหว่างสอง threshold ที่ส่งให้คนตรวจซ้ำ |
| ขอบบน 95% (upper bound) | ค่าสูงสุดของ error rate จริงที่ยังเข้ากับผลที่วัดได้ ที่ความมั่นใจ 95% — ใช้ค่านี้เทียบ spec ไม่ใช่ค่าที่วัดได้ตรง ๆ |
| Clopper–Pearson | วิธีคำนวณ interval แบบ exact ของสัดส่วน binomial (rule of three คือกรณีประมาณของมันเมื่อ error = 0) |
| Optimistic bias | ตัวเลขดีเกินจริงเพราะเลือก threshold/hyperparameter บนชุดเดียวกับที่ใช้รายงานผล |
| OR rule (piece-level aggregation) | reject ทั้งชิ้นถ้าภาพใดภาพหนึ่งถูกทำนายว่าเสีย → FR ต่อชิ้นขึ้น, FA ต่อชิ้นลง |
