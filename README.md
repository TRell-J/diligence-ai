# DiligenceAI — Deal Intelligence

An AI-assisted due diligence summarizer that turns raw deal documents (CIMs, management decks, financial commentary, market notes) into a structured investment memo with risk flags, data gaps, diligence questions, and recommended next steps.

**Live demo:** https://trell-j.github.io/diligence-ai/

Built as a self-contained, single-page web app that runs entirely in the browser — no backend, no server-side data handling, and no exposed credentials. Designed to demonstrate practical AI tooling for investment diligence, strategy, and operations work.

---

## Overview

DiligenceAI compresses the early stages of investment review into a single workflow:

1. Drop in source material (file or pasted text).
2. Choose a focus (risk-weighted, growth-weighted, neutral, etc.).
3. Generate a structured memo with the sections an investor actually reads.
4. Export to Markdown or PDF, or copy individual sections into your own deal doc.

It is intended as a **first-pass tool** for analysts, associates, and operators — accelerating triage, surfacing gaps early, and producing a defensible starting point rather than replacing primary diligence.

---

## Key features

- **Structured diligence memo schema.** Output is organized into Overview, Full Memo, Risk Flags, Data Gaps, Diligence Questions, and Next Steps — not free-form prose.
- **Real document parsing in the browser.** PDF (via PDF.js), DOCX (via Mammoth), and plain-text inputs are parsed client-side. A 15 MB per-file cap keeps the app responsive on commodity hardware.
- **Focus tagging.** Toggle emphasis on Risk Flags, Data Gaps, growth thesis, market sizing, or unit economics to shape the memo for the deal stage.
- **Sample scenarios.** One-click examples for SaaS Growth Equity, Consumer Marketplace, and Healthcare Services so reviewers can see meaningful output without supplying their own data.
- **Bring-your-own model.** Pluggable LLM provider (Groq Llama 3.3, Google Gemini 2.0 Flash, OpenRouter, Mistral). All free or low-cost tiers; the user supplies their own key.
- **Export tooling.** Download the memo as Markdown or PDF, or copy individual sections to the clipboard.
- **Dark, SaaS-style UI.** Custom logo mark, Inter / JetBrains Mono typography, responsive layout, and accessible focus styling.
- **Static-hosting compatible.** Deploys cleanly to GitHub Pages — a single `index.html` with CDN dependencies, no build step.

---

## How it works

1. **Ingest.** The user uploads a PDF / DOCX / TXT file or pastes text directly. Files are read in the browser; nothing is uploaded to a backend.
2. **Parse.** PDF.js extracts a text layer page-by-page (capped at 120 pages). Mammoth converts DOCX to plain text. Plain text and Markdown flow through unchanged.
3. **Prompt.** The extracted text plus the user's focus selections are composed into a structured prompt that asks the model to return a JSON-shaped memo.
4. **Render.** Output is normalized into a typed schema and rendered into tabs (Overview, Full Memo, Risk Flags, Data Gaps, Diligence Questions, Next Steps).
5. **Export.** The memo can be downloaded as Markdown, exported to PDF via jsPDF, or copied section-by-section.

---

## Technology stack

| Layer | Tool |
| --- | --- |
| Markup / UI | Single-file HTML + vanilla CSS (CSS variables, dark + light themes) |
| Logic | Vanilla JavaScript, no framework or bundler |
| Icons | [Lucide](https://lucide.dev/) |
| PDF parsing | [PDF.js](https://mozilla.github.io/pdf.js/) |
| DOCX parsing | [Mammoth](https://github.com/mwilliamson/mammoth.js) |
| PDF export | [jsPDF](https://github.com/parallax/jsPDF) |
| LLM providers | Groq, Google Gemini, OpenRouter, Mistral (user-supplied API key) |
| Hosting | GitHub Pages (static) |

All third-party libraries are loaded from public CDNs at runtime.

---

## Use cases

- **Investment screening.** First-pass triage of inbound CIMs and teasers.
- **Deal team prep.** Generate the skeleton of an IC memo before diving into primary diligence.
- **Operator review.** Summarize a target's product, GTM, and ops posture from a single deck.
- **Strategy / consulting.** Convert client decks and market reports into a structured working document.
- **Learning tool.** Show students and junior team members what a diligence output should look like.

---

## Sample workflow

1. Open the [live app](https://trell-j.github.io/diligence-ai/).
2. Pick a provider in the settings panel and paste your API key (kept client-side).
3. Click a sample scenario (e.g. *SaaS Growth Equity*) — or upload your own CIM / deck.
4. Toggle focus tags such as *Risk Flags* or *Unit Economics*.
5. Run the analysis. Review the Overview tab first, then drill into Risk Flags and Data Gaps.
6. Export the memo to Markdown or PDF, or copy sections into your own deal template.

---

## Security and privacy

- **No backend.** The app is a single static page. There is no server collecting documents, prompts, or keys.
- **No exposed credentials.** The repository contains no API keys. The user supplies their own key at runtime, and it is held only in browser memory for the session.
- **Documents stay local.** Parsing happens client-side; the only network call is the request to the LLM provider the user chose, containing the prompt and extracted text.
- **Provider transparency.** The active provider, model, and endpoint are surfaced in the UI.

Users should still treat any LLM API call as third-party data processing and avoid pasting material covered by an NDA or data-room restriction without authorization.

---

## Limitations

- **Scanned PDFs.** Image-only PDFs without an embedded text layer will not extract content. OCR is not bundled.
- **Long PDFs.** PDF text extraction is capped at the first 120 pages to keep parsing predictable in-browser.
- **PDF export fidelity.** The PDF export is a text-layout render via jsPDF and is not pixel-perfect against the on-screen view.
- **CDN dependency.** Parsing and export rely on libraries loaded from public CDNs; the app requires a live internet connection on first load.
- **Model variability.** Output quality depends on the chosen provider and model. Always validate against source documents before relying on any conclusion.
- **First-pass only.** DiligenceAI is a triage and drafting aid, not a substitute for primary diligence, financial modeling, or legal review.

---

## Local setup

No build step is required.

```bash
git clone https://github.com/TRell-J/diligence-ai.git
cd diligence-ai
# open index.html directly, or serve locally:
python3 -m http.server 8000
# then visit http://localhost:8000
```

To deploy your own copy, push to a GitHub repository and enable GitHub Pages on the `main` branch.

---

## Roadmap

Possible future enhancements:

- OCR fallback for scanned PDFs (e.g. Tesseract.js).
- Multi-document ingestion with cross-document reconciliation.
- Customizable memo templates per deal type (SaaS, marketplace, services, healthcare).
- Persistent project workspaces (local storage / IndexedDB) for multi-session reviews.
- Side-by-side source highlighting that links memo claims back to the originating page.
- Optional self-hosted proxy for teams that cannot send documents to a public LLM API.

---

## Author

**Terrell Johnson** — strategy and operations leader focused on AI prototyping, investment diligence, and product execution.

- GitHub: [@TRell-J](https://github.com/TRell-J)
- Live project: https://trell-j.github.io/diligence-ai/

This project is part of a personal portfolio demonstrating end-to-end ownership of an applied-AI tool: problem framing, schema design, UI/UX, and shipping.
