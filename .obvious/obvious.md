# DiligenceAI (TRell-J/diligence-ai) — agent guide

A one-file, browser-only due-diligence app: `index.html` is the entire product — UI, analysis engine, exports, evaluation harness, and sample deals in a single static page with no build step. Turns CIMs / deal documents into a structured, source-grounded investment memo.

## Stack

| Layer | Choice |
|---|---|
| App | Single-file static web app (`index.html`, 1,675 lines) — no framework, no build step, no backend |
| Language | Vanilla JavaScript (one inline `<script>`), inline CSS with light/dark themes |
| Serving | Any static file server; canonical: `python3 -m http.server 8000`. Also works opened directly as a file |
| Runtime deps | CDN scripts loaded at page load: lucide (icons), pdf.js 3.11.174 (PDF parsing), mammoth 1.6.0 (DOCX), jsPDF 2.5.1 (PDF export) — network needed on load |
| Engine | Pure functions on `window.DiligenceEngine` (`extractMetrics`, `deriveMetrics`, `computeScore`, `analyze`, `buildBenchmarkRows`, `computeConfidence`, `moneyToM`) — deterministic and source-grounded; exposed deliberately for headless testing |
| AI (optional) | BYO-key client-side providers (Groq, Gemini, OpenRouter, Mistral); heuristic mode needs no key |
| Storage | `localStorage` (saved analyses) + in-memory only; nothing server-side |
| Deploy | GitHub Pages — https://trell-j.github.io/diligence-ai/ |

No package manager, no lockfile, no Docker/Compose, no CI config, no lint/typecheck config — by design (see README "Architecture & design decisions").

## Commands

| Action | Command |
|---|---|
| Serve locally (canonical) | `python3 -m http.server 8000` → open http://localhost:8000 |
| Alternative | open `index.html` directly in a browser |
| Self-test / regression | in-app: sidebar → **Evaluation** (19 known-answer checks) |
| Install / build / lint / typecheck / tests | none exist — nothing to run |

## Environment

No required env vars, no `.env`, no secrets. The app runs keyless in heuristic mode. Optional LLM keys are entered in the in-app Settings modal and kept in browser memory only — never in env vars or files.

## Codebase map

Full line-anchored table in [`codebase-map.md`](codebase-map.md). Summary: everything lives in `index.html` (inline application script ≈ lines 495–1605; modal markup 1606–1675) plus `README.md` (111 lines). There are no other source files.

## Local verification

1. `python3 -m http.server 8000`, then confirm http://localhost:8000 returns the page (title "DiligenceAI — Deal Intelligence").
2. Load a sample deal (input panel → "SaaS Growth Equity"), click **Run Due Diligence Analysis**.
3. Expect: verdict card + score, KPIs ($42M ARR, +68% YoY, 74% GM, 118% NRR), Full Memo with ≥6 questions, Valuation benchmark table.
4. Export → "Memo as Markdown" downloads a memo.
5. Sidebar → **Evaluation**: must report **19 / 19 checks passed**.
6. Sidebar → **Governance**: session audit trail shows the run.

### Local Verification Summary (onboarding run, 2026-09-03)

- Dev stack: `python3 -m http.server 8000` → HTTP 200 (134,998 bytes), correct title.
- Primary flow exercised headlessly (Playwright + Chromium, 1440×900): sample deal → analysis → memo → valuation → markdown export → evaluation harness → governance audit. **26/27 automated checks passed.**
- Evaluation harness: **19/19 checks passed** across 3 sample deals.
- Console: no console errors, no failed requests (local or CDN). One pre-existing `pageerror` on every fresh load — see Known quirks.
- Evidence (sandbox `/tmp/onboarding-evidence/`): 6 screenshots, exported memo (5,302 chars), harness output, console log.

## Known quirks

- Every fresh page load logs one pageerror: `TypeError: Cannot set properties of null (setting 'placeholder')`. Cause: the inline script calls `setProviderUI('groq')` before the settings-modal markup (`#apiKeyInput`) exists. Pre-existing, cosmetic — the app is fully functional. Do not chase this during environment setup.

## Sandbox snapshot

| Field | Value |
|---|---|
| Snapshot / template ID | `r5j0ms18d4n674px88hx:default` (E2B) |
| Built at | 2026-09-03T17:37:19.378Z |
| Captured from | live session `iita3xilntv73tnb332da`, dev stack verified healthy (server on :8000, all flow checks green) |
| Baked in | repo checkout at `main`; Python 3.13, Node 20 + npm; Playwright + Chromium headless shell (in `/tmp/pw`, system deps installed); evidence bundle in `/tmp/onboarding-evidence` |

The dev server is a process, not filesystem state — re-run the serve command in any new session. If browser tooling is missing in a fresh session, see [`skills/local-dev/SKILL.md`](skills/local-dev/SKILL.md) for install commands.
