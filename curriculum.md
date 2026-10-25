# Vision Tutor — หลักสูตร AI Vision สำหรับงานตรวจตำหนิในโรงงาน

**เป้าหมาย:** ภายใน 2–4 สัปดาห์ Sabig ต้อง
1. เข้าใจและอธิบายได้เองทุกขั้น ตั้งแต่ตั้งโจทย์ → data → model → FR/FA → error analysis → feedback loop → deploy
2. มีโปรเจกต์ `defect-inspect` บน GitHub ที่ตอบ accountabilities ของ JD ได้ พร้อมตัวเลขจริง
3. ผ่าน mock interview เชิงลึกเกี่ยวกับโปรเจกต์ของตัวเอง

**Dataset:** KolektorSDD2 — 3,335 ภาพ (ตำหนิ 356, ดี 2,979), train 2,331 (ตำหนิ 246) / test 1,004 (ตำหนิ 110),
ขนาดราว 230×630, มี pixel mask, CC BY-NC-SA 4.0
เลือกเพราะมีทั้งภาพดีและภาพเสีย (วัด FR ได้), สัดส่วนไม่สมดุลแบบสายการผลิต และมี mask (ทำ segmentation ได้ และแปลงเป็น bbox ได้)

---

## วิธีเรียนในแต่ละวัน

1. `/vision-tutor start` → ทบทวน 3 ข้อ → Claude สร้าง lecture ของวันนั้นลง `lectures/`
2. **อ่าน lecture** (30–60 นาที) จดลง `notes/` ด้วยคำพูดตัวเอง
3. `explain-back` → อธิบายกลับ 3 แนวคิด
4. `hands-on` → ลงมือในโปรเจกต์ (Sabig เขียนเอง ติดแล้วใช้ `hint`)
5. `review` → ตรวจตาม checklist → PASS หรือ REVISION
6. `end` → อัปเดต PROGRESS.md

ทุกวันใช้เวลาประมาณ 3–5 ชั่วโมง ★ = แกนหลัก ห้ามตัด

## เส้นทางตามเวลาที่มี

| เวลา | ทำวันไหน |
|---|---|
| **2 สัปดาห์** | เฉพาะวัน ★ (15 วัน) รวม Day 19–20 เป็นวันเดียวได้ |
| **3 สัปดาห์** | ★ ทั้งหมด + Day 10, 12 |
| **4 สัปดาห์** | ครบทุกวัน + สำรองไว้แก้ REVISION |

---

## Module 0 — ตั้งโจทย์ให้ถูก (Day 1–3)

### Day 01 ★ Industrial visual inspection และ FR/FA
- **แนวคิด:** องค์ประกอบของระบบ AOI (แสง กล้อง handling decision), positive class, confusion matrix ภาษาโรงงาน,
  FR (overkill) / FA (escape), cost asymmetry, operating point, rule of three สำหรับ rare event
- **Hands-on:**
  1. ตั้ง environment, `torch.cuda.is_available()`, บันทึก GPU ลง PROGRESS
  2. `git init` repo `defect-inspect` และสร้าง repo บน GitHub
  3. เขียน `docs/problem_spec.md` จาก **requirement จำลองของลูกค้า** ด้านล่าง
- **Requirement จำลอง (จากฝ่าย Quality):**
  > - ชิ้นงานเสียหลุดไปถึงลูกค้าได้ไม่เกิน 1 ใน 200 ชิ้นเสีย
  > - ตีกลับชิ้นดีได้ไม่เกิน 5% เพราะมีพนักงานตรวจซ้ำ
  > - ตัดสินผลต่อชิ้นภายใน 100 ms
  > - รอยที่เล็กมากจนมองไม่เห็นด้วยตาเปล่าไม่นับว่าเสีย
- **เกณฑ์ผ่าน:** `problem_spec.md` มีครบ: positive class, หน่วยการตัดสิน, นิยาม FR/FA พร้อมตัวหาร,
  นโยบายเลือก threshold, คำนวณว่า test set ของ KSDD2 ยืนยัน requirement ข้อ 1 ได้หรือไม่ (rule of three),
  data requirement ที่ต้องใช้จริง, และคำถามที่ต้องถามลูกค้ากลับอย่างน้อย 3 ข้อ
- **JD:** Translate customer requirements · Balance FR/FA

### Day 02 ★ ภาพในฐานะข้อมูล และ CNN ที่จำเป็น
- **แนวคิด:** pixel/tensor/channel, normalization, convolution, receptive field, pooling, feature hierarchy,
  transfer learning และทำไม ImageNet weights ช่วยงานโรงงานได้
- **Hands-on:** ดาวน์โหลด KSDD2 → `notebooks/01_eda.ipynb`: จำนวนภาพต่อคลาส, ขนาดภาพ,
  การกระจายของพื้นที่ mask, ตำแหน่งตำหนิ, ตัวอย่างภาพยาก 10 ภาพ
- **เกณฑ์ผ่าน:** EDA ตอบได้ว่าตำหนิเล็กสุด/ใหญ่สุดกี่ pixel, กระจายตัวที่ไหน และมีอะไรที่จะกระทบการออกแบบ
- **JD:** Design image datasets

### Day 03 ★ งาน 3 แบบ และ metric ที่ถูกกับงาน
- **แนวคิด:** classification / detection / segmentation, IoU, precision/recall, PR curve, AP/mAP,
  Dice กับ IoU ของ mask, metric ระดับภาพ vs วัตถุ vs pixel, เลือก metric ตาม use case
- **Hands-on:** `src/defect_inspect/eval/metrics.py` เขียนเองตั้งแต่ศูนย์: confusion matrix, FR, FA,
  precision, recall, IoU ของ box, Dice/IoU ของ mask + `tests/test_metrics.py` เทียบกับ scikit-learn
- **เกณฑ์ผ่าน:** test ผ่าน รวม edge case (mask ว่างทั้งคู่, ไม่มี positive)
- **JD:** Test and evaluate using proper metrics per use case
- **Quiz 1** (Day 1–3)

---

## Module 1 — Data (Day 4–7)

### Day 04 ★ ออกแบบ dataset และ split ที่ไม่ leak
- **แนวคิด:** train/val/test มีไว้ทำอะไร, leakage แบบต่าง ๆ, stratification, group split,
  ห้ามแตะ test จนจบ, การกัน batch "production จำลอง" ไว้สำหรับ feedback loop
- **Hands-on:** `src/defect_inspect/data/split.py` → `data/manifests/{train,val,prodsim,test}.csv`
  (ใช้ official test เป็น test, แบ่ง official train เป็น train/val/prodsim) + `docs/data_card.md`
- **เกณฑ์ผ่าน:** split ทำซ้ำได้ (seed), สัดส่วนตำหนิคงที่ทุกชุด, มี test ตรวจว่าไม่มีภาพซ้ำข้ามชุด
- **JD:** Design and manage image datasets

### Day 05 ★ Labeling standard และคุณภาพ label
- **แนวคิด:** labeling guideline, ขอบตำหนิ, กรณีกำกวม, inter-annotator agreement
  (Cohen's kappa สำหรับระดับภาพ, IoU/Dice ระหว่างผู้ label), label noise และผลต่อโมเดล
- **Hands-on:** เขียน `docs/labeling_guideline.md` → label ใหม่เอง 50 ภาพใน CVAT หรือ Label Studio →
  คำนวณ agreement กับ label ต้นฉบับ → รายงานกรณีที่ไม่ตรงกัน
- **เกณฑ์ผ่าน:** guideline มีนิยาม, ตัวอย่างภาพ OK/NG/กำกวม, กฎการลากขอบ, และตัวเลข agreement พร้อมการตีความ
- **JD:** Govern data labeling standards

### Day 06 การจัดการข้อมูลภาพและฐานข้อมูล
- **แนวคิด:** metadata, manifest, dataset versioning, schema สำหรับภาพในโรงงาน (lot, line, camera, เวลา, label version)
- **Hands-on:** SQLite schema + script นำเข้า manifest + query ตัวอย่าง (เช่น ภาพเสียที่ mask เล็กกว่า X ใน split val)
- **เกณฑ์ผ่าน:** schema รองรับการ label ใหม่หลายเวอร์ชันโดยไม่ทับของเก่า
- **JD:** Design and manage databases · Manage data collection

### Day 07 ★ Preprocessing และ augmentation
- **แนวคิด:** augmentation ไหนสมจริงกับสายการผลิต (แสง, ตำแหน่ง, การหมุน), ทำ augment ให้ภาพกับ mask ตรงกัน,
  interpolation ของ mask, oversampling vs class weight
- **Hands-on:** `src/defect_inspect/data/augment.py` ด้วย Albumentations + grid ภาพก่อน/หลังพร้อม mask ซ้อน
- **เกณฑ์ผ่าน:** อธิบายเหตุผลของ augmentation ทุกตัว และพิสูจน์ด้วยภาพว่า mask ยังตรง
- **JD:** Preprocessing and augmentation to improve diversity
- **Quiz 2** (Day 4–7)

---

## Module 2 — Model และ FR/FA (Day 8–13)

### Day 08 ★ Classification baseline
- **แนวคิด:** training loop ใน PyTorch (forward, loss, backward, optimizer, scheduler), transfer learning,
  class imbalance (weighted BCE, focal loss), overfitting, early stopping
- **Hands-on:** `src/defect_inspect/train_cls.py` เขียน loop เอง (ไม่ใช้ high-level trainer) เทรน ResNet18/EfficientNet-B0
  ระดับภาพ, บันทึก config + seed + ผลลง `runs/`
- **เกณฑ์ผ่าน:** มีกราฟ train/val loss, AUROC บน val, บันทึก run ใน Results log
- **JD:** Develop and train models for classification

### Day 09 ★ Segmentation
- **แนวคิด:** encoder–decoder, U-Net, skip connection, BCE + Dice loss, การแปลงผล pixel เป็นการตัดสินระดับภาพ
  (max prob, พื้นที่ขั้นต่ำ)
- **Hands-on:** `train_seg.py` ด้วย segmentation-models-pytorch + `models/` + กฎ aggregation เป็น image score
- **เกณฑ์ผ่าน:** Dice บน val + เปรียบเทียบ image-level AUROC กับ Day 08
- **JD:** Segmentation

### Day 10 Detection
- **แนวคิด:** bbox regression, anchor-free, NMS, YOLO vs RT-DETR, การแปลง mask เป็น bbox
- **Hands-on:** script แปลง mask → YOLO labels (Claude ช่วยได้) → เทรน Ultralytics → เทียบกับ seg
- **เกณฑ์ผ่าน:** อธิบายได้ว่างานนี้ detection ได้หรือเสียเปรียบ seg ตรงไหน
- **JD:** Detection

### Day 11 ★ Operating point: FR/FA
- **แนวคิด:** score distribution ของ OK/NG, threshold sweep, ROC/DET curve, constraint-first
  (ล็อก FA แล้วลด FR), cost-based, Clopper–Pearson interval, ทำไมห้ามเลือก threshold บน test
- **Hands-on:** `src/defect_inspect/eval/operating_point.py` → เลือก threshold บน val ตาม `problem_spec.md`
  → รายงานผลบน test ครั้งเดียว พร้อม confidence interval → `docs/reports/operating_point.md`
- **เกณฑ์ผ่าน:** มีกราฟ FR vs FA, เหตุผลของ threshold ที่เลือก, และบอกได้ว่าผลยืนยัน spec ได้แค่ไหน
- **JD:** Balance FR/FA · Proper metrics

### Day 12 วินัยในการทดลอง
- **แนวคิด:** ablation, seed variance, การเปรียบเทียบที่ยุติธรรม, experiment tracking
- **Hands-on:** ablation table อย่างน้อย 3 การทดลอง (augmentation on/off, loss, backbone) × 3 seeds
- **เกณฑ์ผ่าน:** ตารางมี mean ± std และสรุปว่าความต่างไหน "จริง"
- **JD:** Optimize and fine-tune

### Day 13 ★ Error analysis
- **แนวคิด:** taxonomy ของ FA/FR, slicing ตามขนาด/ตำแหน่ง/ความสว่าง, Grad-CAM, label error vs model error,
  ตัดสินใจว่าจะแก้ที่ data, model หรือ threshold
- **Hands-on:** `src/defect_inspect/eval/error_analysis.py` + gallery + `docs/reports/error_analysis.md`
- **เกณฑ์ผ่าน:** ทุก FA บน val ถูกจัดหมวด และมีแผนแก้ 3 ข้อเรียงตามผลกระทบ
- **JD:** Error analysis and continuous improvement
- **Quiz 3** (Day 8–13)

---

## Module 3 — ปรับปรุงและ deploy (Day 14–18)

### Day 14 ★ Data feedback loop
- **แนวคิด:** production data → triage (uncertainty, ความเห็นต่างระหว่างโมเดล) → label → retrain,
  active learning, regression test บน golden set, data drift
- **Hands-on:** `src/defect_inspect/feedback/` → จำลองว่า `prodsim` คือข้อมูลจากไลน์ เลือก N ภาพตาม uncertainty
  vs สุ่ม → เปิด label → retrain → เทียบผลบน test เดิม → `docs/reports/feedback_loop.md`
- **เกณฑ์ผ่าน:** มีการเปรียบเทียบ uncertainty vs random ที่จำนวน label เท่ากัน
- **JD:** Build and maintain a data feedback loop

### Day 15 ★ Deployment: ONNX และ latency
- **แนวคิด:** export ONNX, preprocessing parity, FP16, batch vs latency, warm-up, การวัด latency ที่ถูกต้อง (p50/p95)
- **Hands-on:** export → test ว่าผล ONNX Runtime ตรงกับ PyTorch (tolerance) → benchmark (TensorRT ถ้ามี GPU NVIDIA)
- **เกณฑ์ผ่าน:** parity test ผ่าน และตอบได้ว่าผ่าน requirement 100 ms หรือไม่
- **JD:** Support model deployment

### Day 16 Pilot และ monitoring
- **แนวคิด:** shadow mode, pilot → full-scale, monitor FR/FA ในไลน์, เกณฑ์ rollback, model card
- **Hands-on:** `docs/pilot_plan.md` + `docs/model_card.md`
- **JD:** Pilot and full-scale implementation

### Day 17 การทำงานกับ Production / Quality
- **แนวคิด:** คุยกับคนหน้างาน, แปล acceptance criteria เป็นเงื่อนไขการเทรน, repeatability ของโมเดล (แนวคิดคล้าย Gauge R&R)
- **Hands-on:** รายงาน 1 หน้าถึงผู้จัดการฝ่าย Quality (ไม่มีศัพท์ ML เกินจำเป็น)
- **JD:** Collaborate with Production, Quality, and university partners

### Day 18 ★ Final exam
1. ข้อเขียน 20 ข้อ 45 นาที (ผ่าน ≥ 16)
2. อ่านโค้ด: pipeline ~150 บรรทัดที่ซ่อนบั๊ก 5 จุดที่เกิดจริงในงาน vision 30 นาที (ผ่าน ≥ 4 จุด)
3. ปฏิบัติ 90 นาที: รับ model score ของชุดใหม่ → เลือก operating point ตาม spec ใหม่ + รายงาน + interval
   (rubric: ความถูกต้อง 50, เหตุผล 30, การสื่อสาร 20 — ผ่าน ≥ 70 และความถูกต้อง ≥ 40)

---

## Module 4 — สมัครงาน (Day 19–20)

### Day 19 ★ Portfolio
- README ฉบับสุดท้าย (ปัญหา, ผล, FR/FA curve, ข้อจำกัด), resume bullets จากตัวเลขใน Results log เท่านั้น, cover letter ใหม่

### Day 20 ★ Interview
- Mock interview 3 รอบ: technical deep-dive ของโปรเจกต์, ML fundamentals, behavioral/สถานการณ์หน้างาน
- ผ่านเมื่อได้ ≥ 70% ทุกรอบ

---

## Quiz

| Quiz | ครอบคลุม | ผ่าน |
|---|---|---|
| 1 | Day 1–3 ตั้งโจทย์และ metrics | ≥ 8/10 |
| 2 | Day 4–7 data | ≥ 8/10 |
| 3 | Day 8–13 model และ FR/FA | ≥ 8/10 |
| 4 | Day 14–17 feedback loop และ deploy | ≥ 8/10 |

## ประตูก่อนส่งใบสมัคร

- [ ] README มี FR/FA curve, threshold ที่เลือกพร้อมเหตุผล, และข้อจำกัดของการยืนยัน spec
- [ ] มีรายงาน error analysis และ feedback loop
- [ ] ตัวเลขใน resume ทุกตัวอ้าง run ใน Results log ได้
- [ ] Final exam ผ่าน
- [ ] Mock interview ผ่านทั้ง 3 รอบ
