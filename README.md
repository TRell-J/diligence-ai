# DiligenceAI — Deal Intelligence

**An applied-AI prototype that compresses the first pass of investment due diligence — turning raw deal documents (CIMs, management decks, financial commentary) into a structured, source-grounded investment memo with valuation cross-checks, risk and Quality-of-Earnings flags, diligence questions, and a responsible-AI governance layer.**

**Live demo:** [https://trell-j.github.io/diligence-ai/](https://trell-j.github.io/diligence-ai/) · **Runs with no API key** (in-browser heuristic engine), or bring your own LLM key for live analysis.

---

## Why this exists

Deal teams spend the opening days of any transaction reading the same documents and writing the same skeleton memo. That first pass is high-volume, low-leverage, and easy to do inconsistently under deadline. The strategy-and-transactions firms are now investing heavily here.

DiligenceAI is a working, end-to-end answer to one question: *what does a defensible, trustworthy AI first-pass diligence memo actually look like, and how do you build it so a partner can rely on it?* It is deliberately not a chatbot wrapper. The design choices — source grounding, deterministic financial computation, evaluation, and governance — are the point.

It is a **decision-support / triage tool**, not a replacement for primary diligence, financial modeling, or legal review. Every screen says so, and a human-in-the-loop checkpoint is built into the memo.

---

## What it does

1. **Ingest** — drop one or more PDFs / DOCX / TXT files (or paste text). Parsing happens entirely in the browser.  
2. **Apply a lens** — choose the deal type (LBO, growth equity, M\&A, venture, IPO) and sector. The analysis, questions, and benchmarks adapt to it.  
3. **Analyze** — the engine extracts financial metrics, computes valuation and quality ratios, scores the deal, benchmarks it against the sector, and drafts the memo.  
4. **Review** — Overview, Full Memo, Valuation, Risk Flags, Data Gaps, Diligence Questions, and Next Steps — each claim linked back to the source text.  
5. **Export** — Markdown, PDF, clipboard, a locally-saved analysis, or a governance audit log.

---

## What makes it credible (the interesting part)

### Source grounding & hallucination guardrails

Financial metrics, the deal score, and sector benchmarks are computed **deterministically from the source document**, which are never taken from the language model. When live AI is enabled, the model contributes narrative only, and every evidence quote it returns is **verified to appear verbatim in the document**; anything unverifiable is flagged in the UI for manual review. Each extracted metric and risk carries an expandable **source citation**.

### Deal-type-specific analytical lenses

An LBO is read for cash generation, leverage capacity, and EBITDA quality; growth equity for the Rule of 40, retention, and burn; M\&A for synergy realization and integration risk. The diligence questions and benchmarks change with the lens.

### Valuation & quality math

From figures in the document the engine derives **EV/EBITDA, EV/ARR, Rule of 40, and net leverage**, with the calculation shown for each, and benchmarks them against sector reference ranges. It surfaces **Quality-of-Earnings flags** (add-backs, revenue-recognition exposure, concentration) the way an analyst would before pricing.

### A built-in evaluation harness

The **Evaluation** panel runs the extraction and scoring engine against sample deals with **known ground-truth values** every time it is opened — a lightweight regression test (currently **19/19 checks**) that any change to the engine must still pass. This is the discipline that separates a demo from a tool.

### Responsible-AI & governance, by design

- **EU AI Act posture:** treated as a *limited-risk* system, with transparency obligations met (output labelled AI-assisted, unverifiable claims flagged, human kept in the loop).  
- **GDPR / data protection:** heuristic mode processes documents fully in-browser — no upload, no server, no retention. With a BYO key, only the prompt \+ extracted text go to the provider you choose.  
- **Confidentiality gate:** an explicit authorization checkbox blocks any external model call — the control point for NDA / data-room material.  
- **Audit trail:** every analysis is logged in-session (timestamp, mode, model, document hash, score) and exportable as JSON.  
- **Confidence scoring:** each memo reports how much of the verdict is anchored in source data versus inferred.

---

## Architecture & design decisions

| Layer | Choice | Why |
| :---- | :---- | :---- |
| Delivery | Single-file `index.html`, no build step | Zero-friction demo; deploys on GitHub Pages; nothing to host or secure |
| Engine | Vanilla JS, pure functions exposed on `window.DiligenceEngine` | Testable headlessly; the eval harness runs the same code the UI runs |
| Grounding | Deterministic metric extraction \+ computation | Removes the LLM from the numbers — the core trust mechanism |
| AI | Pluggable provider (Groq, Gemini, OpenRouter, Mistral), BYO key | No vendor lock-in; no server-side key custody; free tiers for evaluation |
| Parsing | PDF.js / Mammoth, in-browser | Documents never leave the device in heuristic mode |
| Export | jsPDF \+ Markdown builder | Fits into an existing deal-doc workflow |

**Production path (documented, not required for the demo):** the same engine drops behind a FastAPI service with a retrieval layer over a secured data room, per-tenant isolation, SSO, and persisted audit logs — the in-browser version is the analyst-facing thin client of that architecture.

---

## Running it

No build step.

git clone https://github.com/TRell-J/diligence-ai.git

cd diligence-ai

python3 \-m http.server 8000     \# then open http://localhost:8000

Or just open `index.html`. To use live AI, open **Settings**, pick a provider, and paste your key (kept in browser memory only). To see the engine's self-test, open **Evaluation** in the sidebar.

---

## Limitations (stated plainly)

- First-pass triage and drafting aid only — not primary diligence, a financial model, or legal review.  
- Scanned/image-only PDFs need OCR (not bundled); PDF text extraction is capped at 120 pages.  
- Benchmarks are directional reference points, not a curated comp set.  
- Output quality in live-AI mode depends on the chosen model; the grounded metrics do not.  
- Always validate every figure against source before any figure informs a decision.

---

## Roadmap

- Retrieval over full data rooms with cross-document reconciliation and page-level source linking.  
- Curated, refreshable comp sets per sector.  
- OCR fallback (Tesseract.js) for scanned documents.  
- Optional self-hosted proxy / FastAPI backend for teams that cannot send documents to a public LLM.  
- Expanded evaluation set with adversarial and noisy-document cases.

---

## Author

**Terrell Johnson** — strategy and operations leader focused on applied-AI prototyping, investment diligence, and product execution. This project demonstrates end-to-end ownership of an applied-AI tool: problem framing, schema and engine design, evaluation, responsible-AI, UI/UX, and shipping.

- GitHub: [@TRell-J](https://github.com/TRell-J) · Live project: [https://trell-j.github.io/diligence-ai/](https://trell-j.github.io/diligence-ai/)

