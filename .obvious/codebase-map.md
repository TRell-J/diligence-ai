# Codebase map — TRell-J/diligence-ai

Depth-capped at 2. The repo is deliberately two files; the map is line-anchored into `index.html` so you can jump straight to a subsystem.

| Path | What it is | Notes |
|---|---|---|
| `index.html` | The entire application (1,675 lines) | UI + engine + exports + tests + sample data in one static page |
| ├ lines 1–20 | Head: CDN runtime deps | lucide (icons), pdf.js 3.11.174, mammoth 1.6.0, jsPDF 2.5.1 + pdf.js worker config |
| ├ lines 21–321 | CSS | Light/dark theme tokens, layout, components |
| ├ lines 322–494 | App shell markup | Sidebar, topbar, input panel, output tabs, empty state |
| ├ lines 495–1605 | Inline application script | Everything below |
| │ ├ ~497 | State + file upload/parsing | Multi-doc ingest (PDF/DOCX/TXT/MD), 15 MB cap, provenance hash |
| │ ├ ~682 | `METRIC_PATTERNS` + `extractMetrics` / `deriveMetrics` | Source-grounded metric extraction with citations |
| │ ├ ~744 | `BENCHMARKS` + `DEAL_LENS` | Sector benchmarks; deal-type lenses (LBO, growth, M&A, VC, IPO) |
| │ ├ ~795 | `computeScore` + `qoeFlags` | Deterministic deal scoring, Quality-of-Earnings flags |
| │ ├ ~831 | `analyze()` | Heuristic analyzer — the core entry point |
| │ ├ ~977 | `window.DiligenceEngine` | Pure-function export for headless testing |
| │ ├ ~980 | `PROVIDERS` + `callAI` / `normalizeAI` | BYO-key LLM providers + hallucination guardrail (evidence verified verbatim in source) |
| │ ├ ~1078 | `runAnalysis` orchestration | Confidentiality consent gate, audit recording, loading states |
| │ ├ ~1257 | Export | Markdown / PDF / clipboard / localStorage workspace / audit JSON |
| │ ├ ~1371 | Evaluation harness `runEval()` | 19 known-answer regression checks |
| │ ├ ~1417 | Settings / governance modals | Provider key UI, audit table |
| │ └ ~1456 | `SAMPLES` | 3 sample deal CIMs (SaaS, marketplace, healthcare) |
| ├ lines 1606–1675 | Modal markup | Settings, governance, evaluation, saved analyses |
| `README.md` | Project overview (111 lines) | Rationale, architecture table, run instructions, limitations, roadmap |
| `.obvious/` | Agent contract | This onboarding output |
