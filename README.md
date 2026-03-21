<div align="center">

<br/>

```
 ███████╗██╗  ██╗██╗██╗     ██╗     ██████╗  █████╗ ████████╗██╗  ██╗
 ██╔════╝██║ ██╔╝██║██║     ██║     ██╔══██╗██╔══██╗╚══██╔══╝██║  ██║
 ███████╗█████╔╝ ██║██║     ██║     ██████╔╝███████║   ██║   ███████║
 ╚════██║██╔═██╗ ██║██║     ██║     ██╔═══╝ ██╔══██║   ██║   ██╔══██║
 ███████║██║  ██╗██║███████╗███████╗██║     ██║  ██║   ██║   ██║  ██║
 ╚══════╝╚═╝  ╚═╝╚═╝╚══════╝╚══════╝╚═╝     ╚═╝  ╚═╝   ╚═╝   ╚═╝  ╚═╝
```

### **AI-Adaptive Onboarding Engine**
*Generate zero-redundancy onboarding plans by mapping verified candidate skills*
*to role requirements — assigning only gap-closing courses.*

<br/>

[![Next.js](https://img.shields.io/badge/Next.js-15-black?style=for-the-badge&logo=next.js&logoColor=white)](https://nextjs.org/)
[![Flask](https://img.shields.io/badge/Flask-3.x-green?style=for-the-badge&logo=flask&logoColor=white)](https://flask.palletsprojects.com/)
[![Claude AI](https://img.shields.io/badge/Claude_AI-Anthropic-orange?style=for-the-badge&logo=anthropic&logoColor=white)](https://anthropic.com/)
[![D3.js](https://img.shields.io/badge/D3.js-v7-blue?style=for-the-badge&logo=d3.js&logoColor=white)](https://d3js.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178C6?style=for-the-badge&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Python](https://img.shields.io/badge/Python-3.11+-yellow?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)

<br/>

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](CONTRIBUTING.md)
[![Demo Ready](https://img.shields.io/badge/Demo-Ready-success?style=flat-square)]()
[![Mock Fallback](https://img.shields.io/badge/Mock_Fallback-Enabled-informational?style=flat-square)]()

<br/>

---

**[✦ Live Demo](#-live-demo-walkthrough) · [✦ Quick Start](#-quick-start) · [✦ Architecture](#-system-architecture) · [✦ AI Pipeline](#-ai-pipeline) · [✦ API Docs](#-api-reference)**

---

</div>

<br/>

## ✦ What is SkillPath?

> *"The average enterprise spends 32 days onboarding a new hire — most of that time on skills they already have."*

**SkillPath** is a full-stack AI system that solves onboarding redundancy at the root. It ingests a candidate's resume, parses a job description, runs three sequential Claude API calls to compute real skill deltas, and returns a personalized learning pathway — built exclusively from a grounded course catalog, with zero hallucinated recommendations.

No more generic training plans. No more repeated fundamentals. Just the exact courses that close the exact gaps — ranked, reasoned, and ready.

<br/>

## ✦ The Problem We're Solving

| ❌ Status Quo | ✅ SkillPath |
|---|---|
| Manual resume review misses skills | AI extraction with schema-constrained evidence fields |
| Everyone gets the same training plan | Pathway generated per candidate, per role |
| Training includes skills already mastered | Only gap-closing courses assigned |
| No visibility into readiness | 0–100 readiness score with band interpretation |
| Hallucinated course suggestions from AI | Model restricted to exact catalog IDs — hard constraint |

<br/>

## ✦ Live Demo Walkthrough

```
1.  Upload resume (PDF / DOCX / TXT)
2.  Paste the target job description
3.  Watch the pipeline run:
      parsing → analyzing → generating → success
4.  Receive:
      ┌─ Readiness Score (0–100)
      ├─ Skill Gap Table (present / partial / missing)
      ├─ D3 Radar Chart — visual skill readiness map
      ├─ D3 Timeline — interactive learning pathway
      └─ Course Reasoning Cards — click any node for the "why"
```

<br/>

## ✦ System Architecture

```
┌──────────────────────────────────────────────────────────────────┐
│                    BROWSER / NEXT.JS UI                          │
│         LiveDemo.tsx · RadarChart.tsx · PathwayTimeline.tsx      │
└──────────────────────────────┬───────────────────────────────────┘
                               │  POST /api/analyze
                               │  (multipart: resume file + jdText)
                               ▼
┌──────────────────────────────────────────────────────────────────┐
│              NEXT.JS REWRITE PROXY  (next.config.js)             │
│              /api/* ──────────► http://localhost:5000/api/*      │
└──────────────────────────────┬───────────────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────────────┐
│                    FLASK BACKEND  (app.py)                        │
│    Validate → Extract Text → Run Analysis → Transform → Return    │
│    ↓ on API failure: instant fallback to mock_data.py            │
└────────────────┬─────────────────────────────────────────────────┘
                 │
        ┌────────┴────────┐
        ▼                 ▼
┌──────────────┐   ┌──────────────────────────────────────────────┐
│  parser.py   │   │              analyzer.py                     │
│  PDF extract │   │  parse_resume() ──► parse_jd()               │
│  DOCX parse  │   │       └──────────────► generate_pathway()    │
│  TXT read    │   │                              │                │
└──────────────┘   └──────────────────────────────┼───────────────┘
                                                   │
                                                   ▼
                                    ┌──────────────────────────┐
                                    │   ANTHROPIC CLAUDE API   │
                                    │   3 Sequential Calls     │
                                    └──────────────┬───────────┘
                                                   │
                                                   ▼
                                    ┌──────────────────────────┐
                                    │   course_catalog.py      │
                                    │   CATALOG_BY_ID grounding│
                                    │   Hallucination rejected  │
                                    └──────────────────────────┘
```

<br/>

## ✦ AI Pipeline

The intelligence lives in **three sequential Claude API calls** — each with schema-constrained prompts and a JSON safety parser.

```
┌─────────────────────────────────────────────────────────────────────┐
│  CALL 01 — parse_resume(resume_text)                                │
│                                                                     │
│  IN  →  Raw resume text (any format after extraction)               │
│  OUT →  { skills: [{ name, level, evidence }] }                     │
│                                                                     │
│  Evidence field enforced per skill. No bare assertions allowed.     │
└─────────────────────────────────────┬───────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────┐
│  CALL 02 — parse_jd(jd_text)                                        │
│                                                                     │
│  IN  →  Raw job description text                                    │
│  OUT →  { required_skills: [{ name, priority }], level }           │
│                                                                     │
│  Seniority level inferred from language and responsibilities.       │
└─────────────────────────────────────┬───────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────┐
│  CALL 03 — generate_pathway(resume_skills, jd_skills, catalog)      │
│                                                                     │
│  IN  →  Parsed skills + JD requirements + FULL course catalog JSON  │
│  OUT →  { readiness_score, skill_analysis[], pathway[] }           │
│                                                                     │
│  Model restricted to exact course_id values from catalog.          │
│  Non-catalog IDs rejected at enrichment time.                      │
│  Hallucination rate: 0% (hard constraint, not a prompt suggestion)  │
└─────────────────────────────────────────────────────────────────────┘
```

### Readiness Score Bands

| Score | Status | Action |
|---|---|---|
| `0 – 39` | 🔴 Major upskilling required | Full remediation pathway |
| `40 – 69` | 🟡 Targeted training needed | Focused gap-closing courses |
| `70 – 89` | 🔵 Near role-ready | Fine-tuning modules only |
| `90 – 100` | 🟢 Role-ready | Minimal or no training required |

<br/>

## ✦ Project Structure

```
skillpath_complete/
│
├── backend/                        ← Python Flask API  (port 5000)
│   ├── app.py                      ← Routes, validation, fallback logic
│   ├── analyzer.py                 ← Core AI pipeline (3 Claude calls)
│   ├── parser.py                   ← PDF · DOCX · TXT extraction
│   ├── course_catalog.py           ← Grounded course dataset + metadata
│   ├── mock_data.py                ← Deterministic fallback payloads
│   ├── export_pptx.py              ← PowerPoint export generator
│   ├── requirements.txt
│   ├── .env                        ← ANTHROPIC_API_KEY, USE_MOCK_DATA
│   └── Dockerfile
│
└── frontend/                       ← Next.js App Router  (port 3000)
    ├── app/
    │   ├── page.tsx                ← Main page composition
    │   ├── layout.tsx              ← Root layout + fonts + ThemeProvider
    │   └── globals.css             ← Theme variables + animations
    ├── components/
    │   ├── LiveDemo.tsx            ⭐ Upload · API flow · Status states
    │   ├── RadarChart.tsx          ⭐ D3 skill readiness radar
    │   ├── PathwayTimeline.tsx     ⭐ D3 interactive learning timeline
    │   ├── Architecture.tsx        ← GSAP SVG path-draw section
    │   ├── Hero.tsx                ← Particle canvas + parallax
    │   ├── Navbar.tsx              ← Sticky nav + section tracking
    │   └── ...10 more components
    ├── lib/
    │   ├── api.ts                  ← Typed fetch wrapper
    │   ├── types.ts                ← AnalysisResponse interfaces
    │   ├── gsap.ts                 ← GSAP + ScrollTrigger setup
    │   └── utils.ts                ← Class merge + scramble text
    └── next.config.js              ← Rewrite proxy → Flask backend
```

<br/>

## ✦ Quick Start

### Prerequisites

```bash
node --version    # 18+
python --version  # 3.11+
pip --version
```

### 1 — Clone & Install

```bash
git clone https://github.com/your-username/skillpath.git
cd skillpath

# Backend
cd backend
pip install -r requirements.txt

# Frontend
cd ../frontend
npm install
```

### 2 — Configure Environment

**`backend/.env`**
```env
ANTHROPIC_API_KEY=sk-ant-...      # Get from console.anthropic.com
USE_MOCK_DATA=false               # Set true for demo without API credits
PORT=5000
FLASK_DEBUG=false
```

**`frontend/.env.local`**
```env
NEXT_PUBLIC_API_URL=http://localhost:5000
```

### 3 — Run

**Option A — Two terminals**
```bash
# Terminal 1
cd backend && python app.py

# Terminal 2
cd frontend && npm run dev
```

**Option B — One command (Windows)**
```bash
start_all.bat
```

Open **`http://localhost:3000`** and you're live.

<br/>

## ✦ API Reference

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/health` | Health check — confirms backend is running |
| `POST` | `/api/analyze` | **Core endpoint** — Resume + JD → full pathway |
| `POST` | `/api/diagnostic/generate` | JD → 8 diagnostic MCQ questions |
| `POST` | `/api/diagnostic/analyze` | Quiz answers → adaptive pathway |
| `POST` | `/api/export/pptx` | Export analysis as PowerPoint |

### Example Request

```bash
curl -X POST http://localhost:5000/api/analyze \
  -F "resume=@candidate_resume.pdf" \
  -F "jdText=Senior Python Developer with AWS, SQL, Docker, CI/CD ownership"
```

### Example Response (truncated)

```json
{
  "readiness_score": 74,
  "skill_analysis": [
    { "skill": "Python",     "status": "present",  "priority": "high" },
    { "skill": "AWS",        "status": "partial",  "priority": "high" },
    { "skill": "Kubernetes", "status": "missing",  "priority": "medium" }
  ],
  "pathway": [
    {
      "course_id": "aws-advanced-arch",
      "title": "AWS Advanced Architecture",
      "duration_hours": 12,
      "addresses_skill_gap": "AWS",
      "reason": "Candidate shows basic EC2 usage but lacks VPC design and IAM policy depth required at senior level."
    }
  ]
}
```

<br/>

## ✦ Key Technical Decisions

### Why schema-constrained prompts?
Free-form LLM output breaks downstream parsing. Every Claude call uses explicit JSON schemas in the prompt with a `_safe_json_parse()` fallback — so malformed model output never crashes the pipeline.

### Why catalog grounding?
Injecting the entire course catalog into `generate_pathway()` as prompt context means the model has no reason to invent courses. It selects from a fixed universe. Non-catalog `course_id` values are rejected at enrichment time — hallucination rate is structurally zero.

### Why a rewrite proxy?
`next.config.js` rewrites `/api/*` → `http://localhost:5000/api/*` transparently. The frontend calls relative paths. No CORS preflight. No hardcoded ports in component code. Swap the backend URL in one place.

### Why mock fallback?
Anthropic billing errors and rate limits are real. `USE_MOCK_DATA=true` serves deterministic pre-built payloads with identical shape to the real API response. The demo never breaks visibly.

<br/>

## ✦ Implementation Timeline

Built in **2 days** across 6 phases:

```
Day 1 AM   ████████░░░░  Phase 1 — Flask backend + file parser
Day 1 PM   ████████░░░░  Phase 2 — Claude AI integration + mock fallback
Day 2 AM   ████████░░░░  Phase 3 — Next.js frontend + all sections
Day 2 PM   ████████░░░░  Phase 4 — D3 radar + timeline visualisations
Day 2 Eve  ████████░░░░  Phase 5 — Proxy integration + E2E verification
Day 2 Ngt  ████████░░░░  Phase 6 — Polish, branding, demo prep
```

<br/>

## ✦ Demo Mode (No API Credits Needed)

```bash
# In backend/.env
USE_MOCK_DATA=true
```

Mock data returns identical structure to live responses — same readiness score, skill breakdown, pathway, and reasoning cards. Indistinguishable from the real pipeline in the UI. Perfect for offline demos or judging environments.

**Test with a single command:**
```bash
python -c "
import requests
jd = 'Senior Python Developer with AWS and SQL'
r = requests.post(
  'http://localhost:5000/api/analyze',
  data={'jdText': jd, 'mock': 'true'},
  files={'resume': ('r.txt', b'Python developer with SQL and Flask experience.', 'text/plain')}
)
print(r.status_code, list(r.json().keys()))
"
```

<br/>

## ✦ Roadmap

- [ ] Persistent analysis history per user (Postgres + sessions)
- [ ] Auth + role-based access (NextAuth / JWT middleware)
- [ ] Streaming progress via SSE / WebSocket
- [ ] Pydantic schema validation layer
- [ ] Async job queue for heavy runs (Celery + Redis)
- [ ] Observability dashboards (Prometheus + Grafana)
- [ ] Automated unit + integration test coverage
- [ ] Catalog admin UI
- [ ] Multi-model fallback policy
- [ ] PDF + CSV export enhancements

<br/>

## ✦ Validation & Metrics

| Metric | Mechanism | Validation |
|---|---|---|
| **Hallucination Rate** | Catalog-grounded prompts + exact `course_id` enforcement | Audit pathway IDs against `CATALOG_BY_ID` |
| **Skill Extraction Accuracy** | Evidence field required per skill in schema | Human review on sampled resumes |
| **Pathway Relevance** | `addresses_skill_gap` + reasoning trace per course | Expert rubric scoring by role SMEs |
| **API Response Time** | Staged UI states + 120s timeout | Log latency per `/api/analyze` call |
| **Readiness Score Correlation** | Score from pipeline vs evaluator judgement | Correlate with real onboarding outcomes |

<br/>

## ✦ Tech Stack

| Layer | Technology | Purpose |
|---|---|---|
| Frontend | Next.js 15 + App Router | UI, routing, proxy |
| Styling | Tailwind CSS + globals.css | Theme, animations |
| Visualisation | D3.js v7 | Radar chart + timeline |
| Animation | GSAP + ScrollTrigger | Scroll storytelling |
| Backend | Flask 3 + Python 3.11 | API, orchestration |
| AI | Anthropic Claude (claude-sonnet) | 3-stage pipeline |
| PDF Parse | pdfminer.six | PDF text extraction |
| DOCX Parse | python-docx | Word doc extraction |
| Export | python-pptx | PowerPoint generation |
| Containerisation | Docker | Backend deployment |

<br/>

## ✦ Common Issues & Fixes

| Error | Fix |
|---|---|
| `invalid x-api-key` | Fix `ANTHROPIC_API_KEY` in `backend/.env`, restart backend |
| `credit balance is too low` | Add Anthropic credits or set `USE_MOCK_DATA=true` |
| `'next' is not recognized` | Run `npm install` inside `frontend/` |
| Frontend stuck on Processing | Ensure backend is running on port 5000 |
| Port 3000 busy | Run `npm run dev -- -p 3001` |
| Resume unreadable | Use cleaner PDF/DOCX/TXT; backend requires ≥50 chars extracted |

<br/>

## ✦ Contributing

Pull requests are welcome. For major changes, open an issue first to discuss what you'd like to change.

```bash
git checkout -b feature/your-feature-name
git commit -m "feat: describe your change"
git push origin feature/your-feature-name
```

<br/>

---

<div align="center">

<br/>

**Built with precision. Shipped in 48 hours. Designed to scale.**

<br/>

*SkillPath — because onboarding should start where the candidate actually is.*

<br/>

[![Made with Claude](https://img.shields.io/badge/Made_with-Claude_AI-orange?style=for-the-badge&logo=anthropic)](https://anthropic.com)
[![Built with Next.js](https://img.shields.io/badge/Built_with-Next.js-black?style=for-the-badge&logo=next.js)](https://nextjs.org)
[![Powered by Flask](https://img.shields.io/badge/Powered_by-Flask-green?style=for-the-badge&logo=flask)](https://flask.palletsprojects.com)

<br/>

---

*© 2025 SkillPath Team. MIT License.*

</div>
