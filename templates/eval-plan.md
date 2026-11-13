# Eval Plan — [ชื่อระบบ]

## 1. ระบบนี้ต้อง "ถูก" หมายถึงอะไร
| มิติคุณภาพ | นิยาม | น้ำหนัก | วิธีวัด |
|---|---|---|---|
| ความถูกต้องของเนื้อหา | | 40% | exact / F1 / judge |
| อ้างอิงถูกต้อง (citation) | | 20% | ตรวจ citation ชี้ถูก |
| ครบถ้วน | | 15% | judge |
| ปฏิเสธเมื่อไม่มีข้อมูล | | 15% | exact ("ไม่พบข้อมูล") |
| ความปลอดภัย (ไม่ทำสิ่งต้องห้าม) | | 10% | red team suite |

## 2. Golden set
| ประเภท | จำนวน | เป้า |
|---|---|---|
| Happy path | | |
| Edge case | | |
| Abstention | | |
| Adversarial | | |
| Ambiguous | | |
| **รวม** | ≥50 | |

**รูปแบบไฟล์:** `evals/golden.jsonl`
```jsonl
{"id":"e001","input":"...","expected":"...","tags":["happy_path","th"]}
```

## 3. วิธีให้คะแนน
| งาน | วิธี | หมายเหตุ |
|---|---|---|
| สกัดข้อมูล | exact match / regex | เข้มงวด |
| ตอบคำถาม | LLM-as-judge ตาม rubric | validate ด้วยคน |
| RAG retrieval | Recall@5, MRR | แยกวัดจาก answer |
| Agent | task success (end-to-end) | + tool selection accuracy |

## 4. Judge rubric
```
5 = ...
4 = ...
3 = ...
2 = ...
1 = ...
0 = ...
ตอบ JSON: {"score": int, "reason": str, "issues": [str]}
```

**Judge validation:** สุ่ม ___ เคส ให้คนให้คะแนน → agreement = ___% (ต้อง ≥80%)

## 5. เกณฑ์ผ่าน / gate
| Metric | Baseline | เกณฑ์ผ่าน | Regression threshold |
|---|---|---|---|
| overall | | ≥0.80 | -3% block |
| abstention | | ≥0.75 | block |
| adversarial | | ≥0.60 | block |
| cost/req | | | +30% warn |

## 6. การรัน
```bash
python evals/run.py --system <name> --concurrency 4
```
ผลออกที่ `evals/reports/<name>.json`

## 7. Observability ที่ต้องมีใน production
| สิ่งที่เก็บ | ทำไม |
|---|---|
| trace_id, prompt_version, model | debug + เปรียบเทียบ |
| tokens + cost | คุมงบ |
| latency แยกชั้น | หาคอขวด |
| chunks ที่ retrieve | debug RAG |
| guardrail hits | ความปลอดภัย |
| user feedback | flywheel |

## 8. Feedback loop
- ปุ่ม 👍/👎 + เหตุผล
- ทุกสัปดาห์: แปลง 👎 เป็น eval case ใหม่
- ผู้รับผิดชอบ: ______
- เป้า: เพิ่ม eval case ≥5/สัปดาห์
