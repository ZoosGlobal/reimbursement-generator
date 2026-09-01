# Employee Reimbursement PDF Generator

A browser-only reimbursement claim builder for **Zoos Global** (Style E-Trademark Pvt. Ltd.). Employees enter their claim details and expenses, attach receipts, and generate a single ordered, letterhead-style PDF — entirely client-side, no backend required.

## Features

- **Claim details** — Employee Name, Department, Claim Month (month/year picker), Submission Date (DD-MM-YYYY), with Employee Name and Department remembered automatically via `localStorage` on future visits.
- **Expense entry** — Category, Expense Date, Description/Business Purpose, Currency (INR/USD), Amount, and INR Amount. For non-INR currencies you enter the original amount and the actual INR amount directly (no conversion-rate calculation).
- **Receipts** — attach one or more image (JPG/PNG/WEBP) or PDF receipts per expense, either while composing the expense or afterward from the expense table. Each receipt can be previewed and removed individually. PDF receipts are rendered client-side with [PDF.js](https://mozilla.github.io/pdf.js/).
- **Validation** — inline status messaging (not `alert()`) blocks PDF generation until every required claim field is filled, every expense is valid, and every expense has at least one receipt attached.
- **PDF generation** — uses the browser's native print dialog (`window.print()` → "Save as PDF"). The output is a single ordered document:
  1. A letterhead-style first page — claim summary, employee information, the full expense table, category totals, and a receipt-attachments notice.
  2. One page per receipt, in the same order as the expense rows. PDF receipts render as their own native pages (no blank wrapper pages); image receipts get a branded attachment page.
- **Light/dark theme** toggle, persisted per browser.

## Tech stack

Plain HTML, CSS, and JavaScript — no build step, no framework, no bundler. Third-party libraries are loaded from CDN over HTTPS:

- [Flatpickr](https://flatpickr.js.org/) — date picker and month/year picker
- [PDF.js](https://mozilla.github.io/pdf.js/) — renders PDF receipts in-browser
- [Google Fonts](https://fonts.google.com/) — Montserrat (Georgia, used for headings, is a system font)

## Running locally

Just open `index.html` in a browser — no server, install step, or build required.

```bash
open index.html
```

If your browser restricts local file access for any reason, serve it with any static file server, for example:

```bash
python3 -m http.server 8000
```

then visit `http://localhost:8000/`.

## Deploying to GitHub Pages

This repository is ready to deploy as-is:

1. Push this repository to GitHub, under the `zoosglobal` organization as `reimbursement-generator`.
2. In the repo settings, enable **GitHub Pages** for the `main` branch (root directory).
3. The site will be served at **https://zoosglobal.github.io/reimbursement-generator/**, with `index.html` as the entry point.

An internet connection is required at runtime (for the CDN libraries, Google Fonts, and the Zoos Global logo) — the app is browser-only but not fully offline-capable.

## Data & privacy

- Only **Employee Name**, **Department**, and the **theme preference** are persisted, in the browser's `localStorage` (keys `zoos_reimbursement_employee_name`, `zoos_reimbursement_department`, `zoos_reimbursement_theme`). Storage access is wrapped defensively, so the app still works if `localStorage` is unavailable (e.g. private browsing).
- Expenses, receipts, and generated PDFs are **not** persisted anywhere — everything lives only in memory for the current session and is cleared on reload.
- No data is ever sent to a server; there is no backend.
