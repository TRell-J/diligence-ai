---
name: local-dev
description: Bring DiligenceAI up locally and verify the primary analysis flow end-to-end
---

# Local dev — DiligenceAI (TRell-J/diligence-ai)

Recorded during onboarding on 2026-09-03. The app is a single static file — there is nothing to install.

## Environment (as verified in the sandbox)

- Python 3.13 (`python3 -m http.server` — the canonical server)
- Node 20 + npm 10 (needed only for browser-automation verification, not for the app)
- Network required at page load (CDN scripts: lucide, pdf.js, mammoth, jsPDF)
- No env vars, no secrets, no lock files

## Start

```bash
python3 -m http.server 8000   # → http://localhost:8000  (or open index.html directly)
```

Port 8000 confirmed by live response: HTTP 200, 134,998 bytes, title "DiligenceAI — Deal Intelligence".

## Verify the primary flow (browser automation)

1. Install tooling (already baked into the sandbox snapshot; run only if missing):
   ```bash
   mkdir -p /tmp/pw && cd /tmp/pw && npm init -y && npm i playwright
   npx playwright install chromium
   sudo npx playwright install-deps chromium   # passwordless sudo is available in the sandbox
   # launch chromium with { args: ['--no-sandbox'] }
   ```
2. Drive the app: goto http://localhost:8000 → click the sample pill "SaaS Growth Equity" → click `#analyzeBtn` → wait for `.summary-meta .company-name`.
3. Assert: verdict + score rendered; KPIs $42M ARR / +68% YoY / 74% GM / 118% NRR; Full Memo has ≥6 `.question-item`; Valuation has ≥4 `.bench-table tbody tr`; Export → "Memo as Markdown" produces a download starting `# Due Diligence Memo`.
4. Regression: `page.evaluate(() => runEvalUI())` → `.eval-summary` must read **19 / 19 checks passed across 3 sample deals**.
5. Audit: `page.evaluate(() => openGovernance())` → `#auditBody` contains a row for the run.
6. Console: expect zero console errors and zero failed requests. One pre-existing pageerror fires on every fresh load (`TypeError: Cannot set properties of null (setting 'placeholder')` — `setProviderUI('groq')` runs before the settings modal exists in the DOM). It is an app bug, cosmetic and non-blocking; do not treat it as an environment failure.

## Onboarding run results (2026-09-03)

- 26/27 automated checks passed (the single miss is the pre-existing pageerror above).
- Evaluation harness: 19/19. Exported memo: 5,302 chars / 106 lines.
- Evidence in `/tmp/onboarding-evidence/`: 01-empty-state.png, 02-analysis-overview.png, 03-full-memo.png, 04-valuation.png, 05-evaluation-harness.png, 06-governance-audit.png, exported-memo.md, evaluation-harness-output.txt, console-log.json.
- Sandbox snapshot: template `r5j0ms18d4n674px88hx:default`, built 2026-09-03T17:37:19.378Z.
