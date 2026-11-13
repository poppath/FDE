# FDE Study Resources — แหล่งเรียนคัดสรร (~70 รายการ)

> คู่กับ Notion: **FDE Course Sheet — สาย Non-Software Developer** และ **FDE Study Resources**
> จัดกลุ่มตาม 5 Phase · บอกทุกอันว่า "ใช้ทำอะไร" และ "ใช้เวลาเท่าไร"

**กฎการใช้แหล่งเรียน**
1. ดูวิดีโอ/อ่าน ไม่เกิน 50% ของเวลาเรียน
2. ทุก concept ต้องถูกใช้ใน lab ภายใน 7 วัน
3. ถ้าดูจบแล้วเขียนเองไม่ได้ = ยังไม่เรียน

---

## Phase 1 — Technical Floor (80 ชม.)

### 1.1 Internet & HTTP (~6 ชม.)
| แหล่ง | ลิงก์ | ใช้ทำอะไร |
|---|---|---|
| MDN — HTTP overview | https://developer.mozilla.org/en-US/docs/Web/HTTP | มาตรฐานทองของ HTTP (methods, status, headers) |
| MDN — CORS | https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS | ต้นเหตุ error ที่ FDE เจอบ่อยที่สุด |
| The Twelve-Factor App | https://12factor.net/ | หลักการ app ที่รันบน cloud ได้ |

### 1.2 Git & GitHub (~6 ชม.)
| แหล่ง | ลิงก์ | ใช้ทำอะไร |
|---|---|---|
| Pro Git (ไทย) | https://git-scm.com/book/th | บท 1–3 + branch/merge |
| GitHub Skills | https://skills.github.com/ | lab โต้ตอบ (Intro to GitHub, Review PRs) |
| Conventional Commits | https://www.conventionalcommits.org/en/v1.0.0/ | มาตรฐาน commit |

### 1.3 SQL (~14 ชม.)
| แหล่ง | ลิงก์ | ใช้ทำอะไร |
|---|---|---|
| SQLBolt | https://sqlbolt.com/ | เรียนจบในเบราว์เซอร์ 3–4 ชม. (18 บท) |
| Mode SQL Tutorial | https://mode.com/sql-tutorial | SQL สำหรับงานธุรกิจ |
| PostgreSQL Tutorial — Window Functions | https://www.postgresqltutorial.com/postgresql-window-function/ | ส่วนที่คนส่วนใหญ่ขาด |
| Chinook dataset | https://github.com/lerocha/chinook-database | Dataset ฝึก |
| Open Data ไทย | https://data.go.th/ | ข้อมูลจริงของไทย |

### 1.4 Python (~40 ชม.)
| แหล่ง | ลิงก์ | ใช้ทำอะไร |
|---|---|---|
| Python Official Tutorial | https://docs.python.org/3/tutorial/ | บท 1–12 เขียนตามทุกตัวอย่าง |
| Automate the Boring Stuff | https://automatetheboringstuff.com/ | ฟรีทั้งเล่ม (บท 1–9) |
| Exercism — Python | https://exercism.org/tracks/python | มี mentor ฟรี ทำ 20–30 ข้อ |
| Real Python | https://realpython.com/ | logging, testing, pathlib, requests |
| pytest docs | https://docs.pytest.org/ | Getting Started + parametrize + mock |
| Ruff | https://docs.astral.sh/ruff/ | Linter |
| uv | https://docs.astral.sh/uv/ | Package manager ที่เร็ว |

---

## Phase 2 — AI Engineering (120 ชม.)

### 2.1 LLM & Prompt (~30 ชม.)
| แหล่ง | ลิงก์ | ใช้ทำอะไร |
|---|---|---|
| Prompt Engineering Guide (DAIR.AI) | https://www.promptingguide.ai/ | ครอบคลุมที่สุด อัปเดตสม่ำเสมอ |
| OpenAI Cookbook | https://github.com/openai/openai-cookbook | โค้ดจริง: structured output, function calling, RAG, evals |
| OpenAI — Prompt Engineering | https://platform.openai.com/docs/guides/prompt-engineering | อ่านคู่กับ Cookbook |
| Anthropic — Prompt Engineering | https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview | long-context, XML tags |
| Google — Prompting strategies | https://ai.google.dev/gemini-api/docs/prompting-strategies | multimodal (ภาพ/PDF) |
| Karpathy — Intro to LLMs | https://www.youtube.com/watch?v=zjkBMFhNj_g | 1 ชม. เข้าใจ token/context |

### 2.2 RAG & Vector Search (~40 ชม.)
| แหล่ง | ลิงก์ | ใช้ทำอะไร |
|---|---|---|
| pgvector | https://github.com/pgvector/pgvector | คุ้มที่สุด เพราะองค์กรมี Postgres อยู่แล้ว |
| Qdrant Docs | https://qdrant.tech/documentation/ | Vector DB |
| Chroma Docs | https://docs.trychroma.com/ | Vector DB (เริ่มง่าย) |
| LlamaIndex | https://docs.llamaindex.ai/en/stable/ | chunking strategy / node parser |
| LangChain — Retrieval | https://python.langchain.com/docs/concepts/retrieval/ | hybrid, rerank, parent-document |
| Pinecone Learn — RAG | https://www.pinecone.io/learn/retrieval-augmented-generation/ | hybrid search + reranking |
| MTEB Leaderboard | https://huggingface.co/spaces/mteb/leaderboard | เลือก embedding ด้วยข้อมูล (ทดสอบไทยเอง) |
| Docling (IBM) | https://github.com/docling-project/docling | parse PDF/ตาราง/สแกน |
| Unstructured | https://github.com/Unstructured-IO/unstructured | parse เอกสารหลากรูปแบบ |

### 2.3 Agents & Tool Use (~30 ชม.)
| แหล่ง | ลิงก์ | ใช้ทำอะไร |
|---|---|---|
| **Anthropic — Building Effective Agents** ★ | https://www.anthropic.com/research/building-effective-agents | workflow vs agent + เมื่อไรไม่ควรใช้ agent |
| Model Context Protocol (MCP) | https://modelcontextprotocol.io/ | มาตรฐานการต่อ tool (ต้องรู้ปี 2026) |
| OpenAI Cookbook — Agents | https://cookbook.openai.com/ | tool schema ที่ดี vs แย่ |
| LangGraph | https://langchain-ai.github.io/langgraph/ | state / loop / human-in-the-loop |

### 2.4 Eval & Observability ★ (~30 ชม. — สำคัญสุด)
| แหล่ง | ลิงก์ | ใช้ทำอะไร |
|---|---|---|
| **Hamel Husain — Your AI Product Needs Evals** ★★ | https://hamel.dev/blog/posts/evals/ | ถ้าอ่านได้เรื่องเดียว ให้อ่านเรื่องนี้ |
| Hamel — F.A.Q. on LLM Evals | https://hamel.dev/blog/posts/evals-faq/ | ขนาด set, judge, annotation |
| Ragas Docs | https://docs.ragas.io/ | metric: faithfulness, context precision/recall |
| promptfoo | https://www.promptfoo.dev/docs/intro/ | eval + red team ใน CI |
| DeepEval | https://docs.confident-ai.com/ | pytest-style eval |
| Eugene Yan — LLM Evaluators | https://eugeneyan.com/writing/llm-evaluators/ | ข้อจำกัดของ LLM-as-judge |
| Langfuse | https://langfuse.com/docs | Observability (self-host ได้) |
| Arize Phoenix | https://docs.arize.com/phoenix | Observability |
| OpenTelemetry — GenAI semconv | https://opentelemetry.io/docs/specs/semconv/gen-ai/ | มาตรฐาน trace ของ LLM |

---

## Phase 3 — Production & Enterprise (80 ชม.)

### 3.1 Deploy & API (~35 ชม.)
| แหล่ง | ลิงก์ | ใช้ทำอะไร |
|---|---|---|
| FastAPI Tutorial | https://fastapi.tiangolo.com/tutorial/ | ทำถึง Security + Testing |
| Docker — Get Started | https://docs.docker.com/get-started/ | ห่อ service เป็น image |
| GitHub Actions Docs | https://docs.github.com/en/actions | Workflow syntax + Secrets + Matrix |
| Google Cloud Run Quickstarts | https://cloud.google.com/run/docs/quickstarts | deploy ให้มี URL สาธารณะ |
| The Twelve-Factor App | https://12factor.net/ | checklist ก่อนขึ้น production |

### 3.2 Security, Privacy & Compliance (~45 ชม.)
| แหล่ง | ลิงก์ | ใช้ทำอะไร |
|---|---|---|
| **OWASP Top 10 for LLM Apps** ★ | https://genai.owasp.org/llm-top-10/ | 10 ความเสี่ยงที่ CISO จะถาม |
| NIST AI RMF | https://www.nist.gov/itl/ai-risk-management-framework | governance ภาษากลาง |
| PDPC (สคส.) | https://www.pdpc.or.th/ | PDPA ไทย — แหล่งที่ถูกต้องที่สุด |
| NIST SP 800-63 | https://pages.nist.gov/800-63-4/ | authentication/MFA |
| Keycloak | https://www.keycloak.org/documentation | SSO/OIDC ใช้ใน lab ได้ฟรี |

---

## Phase 4 — FDE Craft ★ (100 ชม. — จุดที่คุณชนะ)

### 4.1 Discovery (~35 ชม.)
| แหล่ง | ลิงก์ | ใช้ทำอะไร |
|---|---|---|
| **The Mom Test** ★★ | https://www.momtestbook.com/ | เปลี่ยนวิธีถามคำถามลูกค้า (3 ชม.) |
| Continuous Discovery Habits | https://www.producttalk.org/ | discovery ให้เป็นระบบ |
| Inspired — Marty Cagan | https://www.svpg.com/inspired-how-to-create-products-customers-love/ | คุยกับ product team |
| Jobs-to-be-Done | https://www.intercom.com/blog/jobs-to-be-done/ | ทำให้ problem statement คม |
| Atlassian — Postmortem templates | https://www.atlassian.com/incident-management/postmortem/templates | retrospective |

### 4.2 Delivery & Change Management (~35 ชม.)
| แหล่ง | ลิงก์ | ใช้ทำอะไร |
|---|---|---|
| Google SRE Book (ฟรี) | https://sre.google/sre-book/table-of-contents/ | SLOs, Monitoring, Postmortem |
| Shape Up — Basecamp (ฟรี) | https://basecamp.com/shapeup | ตัด scope / กำหนด appetite |
| Kotter — 8 Steps of Change | https://www.kotterinc.com/methodology/8-steps/ | โครง adoption plan |
| Google re:Work | https://rework.withgoogle.com/ | คู่มือ training + วัดผล |

### 4.3 Commercial & Exec Communication (~30 ชม.)
| แหล่ง | ลิงก์ | ใช้ทำอะไร |
|---|---|---|
| Made to Stick | https://heathbrothers.com/books/made-to-stick/ | ทำให้เรื่องเทคนิคติดหัวผู้บริหาร |
| The Trusted Advisor | https://trustedadvisor.com/ | ความสัมพันธ์ลูกค้าระยะยาว |
| How to Measure Anything — Hubbard | https://hubbardresearch.com/how-to-measure-anything/ | ประเมินสิ่งที่ "ดูเหมือนวัดไม่ได้" (ROI) |

---

## Phase 5 — Expertise & Career (80 ชม.)

### 5.1 Capstone (~60 ชม.)
| แหล่ง | ลิงก์ | ใช้ทำอะไร |
|---|---|---|
| เทมเพลตใน repo นี้ | `templates/` | design doc, eval plan, delivery plan |
| Streamlit Docs | https://docs.streamlit.io/ | UI ให้ผู้ใช้จริงลองได้ใน 1 วัน |
| Fly.io Docs | https://fly.io/docs/ | deploy ราคาถูก |

### 5.2 Positioning & Interview (~20 ชม.)
| แหล่ง | ลิงก์ | ใช้ทำอะไร |
|---|---|---|
| NeetCode 150 | https://neetcode.io/practice | 40 ข้อแรกพอ (FDE ไม่เจอ hard) |
| Tech Interview Handbook | https://www.techinterviewhandbook.org/ | STAR + resume + negotiation |
| System Design Primer | https://github.com/donnemartin/system-design-primer | scalability, caching, DB |
| Pramp | https://www.pramp.com/ | mock interview ฟรีกับคนจริง |

---

## แหล่งเรียนภาษาไทย

> คอร์สอังกฤษคุณภาพดีกว่าและอัปเดตกว่า — แหล่งไทยมีประโยชน์ตอน "ยังไม่กล้าเริ่ม" หรืออยากได้คำอธิบายไทยก่อน

| แหล่ง | ลิงก์ | หมายเหตุ |
|---|---|---|
| Skooldio | https://www.skooldio.com/ | Data/Python/SQL ไทย (Module 1–2) |
| BorntoDev | https://www.borntodev.com/ | programming พื้นฐาน (มีแบบฟรี) |
| DataRockie | https://datarockie.com/ | Data Analyst/SQL ไทย |
| Thai MOOC | https://thaimooc.org/ | คอร์สฟรีจากมหาวิทยาลัยไทย |
| PDPC (สคส.) | https://www.pdpc.or.th/ | เอกสาร PDPA ไทย (Module 9) |

> ⚠️ **ระวัง:** คอร์สไทยหลายตัวล้าสมัยเรื่อง AI (เนื้อหาก่อนปี 2024)
> ใช้สำหรับพื้นฐานเท่านั้น ส่วน AI/LLM/RAG/eval ให้ใช้แหล่งภาษาอังกฤษด้านบน

---

## แหล่งงาน & ชุมชน

- หน้าอาชีพ: OpenAI, Anthropic, Databricks, Google Cloud, Palantir, Scale AI, Cohere
- ชื่อตำแหน่งที่เทียบเท่า FDE: **Applied AI Engineer, Deployment Strategist, Forward Deployed AI Engineer, AI Solutions Engineer, Solutions Architect (AI)**
- Latent Space — https://www.latent.space/ (podcast/blog ที่ดีที่สุดเรื่อง AI engineering)
- Meetup: Bangkok AI, MLOps Thailand, Data Science Thailand

---

## ตารางเรียนที่แนะนำ (Study Cadence)

| ช่วงเวลา | กิจกรรม | หมายเหตุ |
|---|---|---|
| เสาร์ 09:00–12:00 | Lab หลักของสัปดาห์ (3 ชม.) | ห้ามตัด — block สำคัญที่สุด |
| อาทิตย์ 09:00–12:00 | Lab ต่อ + เขียน blog/สรุป (3 ชม.) | เขียนให้คนอื่นอ่านได้ = เข้าใจจริง |
| จ.–ศ. คืนละ 45–60 นาที | อ่าน/ดู concept 1 หัวข้อ (6 ชม./สัปดาห์) | ดูไม่เกิน 30 นาที/คืน ที่เหลือเขียนตาม |
| ศุกร์ 30 นาที | Weekly Review | กรอกตัวเลข + คะแนน 4 มิติ |

> 🧠 **หลักการจำ:** เรียน 1 ชม. → เขียน/ทำ 1 ชม. → สอนหรือสรุป 15 นาที
> ถ้าข้ามขั้นที่ 3 คุณจะลืมภายใน 2 สัปดาห์ (และตอบ interview ไม่ได้)

---

## ลำดับที่แนะนำถ้าเวลาจำกัด (100 ชม. แรก)

```
SQLBolt → Python Tutorial → Prompt Engineering Guide → Hamel Evals
→ Anthropic Building Effective Agents → FastAPI → The Mom Test → Capstone
```

*ควรตรวจลิงก์ปีละ 2 ครั้ง — เอกสารของ vendor เปลี่ยน path บ่อย ถ้าลิงก์เสียให้ค้นชื่อบทความแทน*
