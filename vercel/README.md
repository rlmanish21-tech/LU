# Mock Test App

A fully static, single-file mock test app (`index.html`). No AI, no server,
no API keys needed.

## How it works

- **Host** goes to "Upload Questions", picks a PDF where each question is
  followed inline by its answer (`Ans: (B)`) and ideally an explanation.
  The app extracts and parses the questions, then splits them automatically
  into mocks of 100 questions each — "Mock 1", "Mock 2", "Mock 3", etc. —
  continuing the numbering if you upload more than one PDF over time.
- Because real-world exam PDFs (multi-column layouts, tables, bubble-style
  option markers) can't always be parsed perfectly by a script, every
  detected question is shown in an **editable review screen** before saving
  — fix the question text, options, correct answer, year, or explanation as
  needed, or remove a question entirely, before clicking "Save".
- **Users** go to "Start a Test", enter their name, pick a mock, and take it.
  Results are saved to History, with a full answer review (correct/incorrect/
  skipped, per-question explanation) after each attempt, and a WhatsApp
  share button for the score summary.

## Deploying

This is now a plain static site — no serverless functions, no environment
variables required.

**Vercel:**
```
npm install -g vercel
cd mock-test-app
vercel
```
Accept the defaults (no build command needed) — that's it.

**Any other static host** (Netlify, GitHub Pages, Cloudflare Pages, S3, etc.)
works too — just upload `index.html`.

**Do NOT open it via `file://`** (double-clicking it) — Chrome blocks the
PDF-parsing worker on `file://` pages. Always serve it over `http://` or
`https://`, even locally (`python3 -m http.server` works fine for testing).

## Data storage

Question bank and test history are stored per-visitor in `localStorage`
(or via the app's own `window.storage` API if that's available in your
hosting environment). Each user's data lives only in their own browser —
nothing is synced across users or devices.
