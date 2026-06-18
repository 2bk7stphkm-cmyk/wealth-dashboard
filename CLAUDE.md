# R&R Wealth Dashboard — Project Context

A single-file personal finance + wealth dashboard for a couple (Rahul & Rhea).
Built to run on mobile, hosted on Netlify, with real-time sync via Firebase Firestore.

## Current state (as of 2026-06-18)
- **Whole app is one file:** `index.html` (no build tools, no frameworks; Chart.js + Firebase from CDN).
- **Live at:** https://personalwealthdashboard.netlify.app/ (deployed via Netlify drag-and-drop).
- **Firebase:** config goes in the `FIREBASE_CONFIG` object near the top of the `<script>`. Free Spark plan. Falls back to localStorage if blank. Sync status = the dot top-right (green = synced, red = local only).
- **Security:** Firestore currently in **TEST MODE (open)** — user chose to leave it open for now. Data is on a public URL. TODO before relying on it: add passphrase gate + Firebase Anonymous Auth + a Firestore security rule so only the two of them can read/write.

## Architecture
- All app state lives in one JS object `D` (see `defaultData()`).
- `save()` writes to localStorage and (if connected) Firestore doc at path `wealth/shared`.
- `renderAll()` re-renders every page. Live edits use a debounced `liveSave()` to avoid wiping inputs mid-type.
- Profile toggle (`Rahul | Both | Rhea`) drives `scopePersons()` / `scopeNW()` etc. to refilter everything.

## 5 pages (bottom nav)
1. **Dashboard** — net worth hero + MoM delta vs last snapshot, 4 metric cards, asset-allocation doughnut, expenses bar, snapshot→history line chart, JSON export/import.
2. **Accounts** — editable bank lists (Rahul 5 / Rhea 3 / joint), Rhea's auto-renew sweep FD.
3. **Investments** — Stocks (Zerodha CSV auto-parse + P&L%), Mutual Funds (CSV + corpus + SIP), EPF (both), PPF (Rahul).
4. **Expenses** — per-person income, Cred-style credit cards (tap → modal for monthly bill), expense buckets, EPF/PPF/SIP auto-pulled, savings-rate bar.
5. **Blueprint** — sliders (investment/return/inflation), nominal vs real growth chart, projection cards, insight text. Corpus seeded from live net worth.

## Design
Dark mode default + light toggle. Glassmorphism cards, navy bg, blue→purple gradient accents, mobile-first, bottom tabs. Indian number formatting (₹ K/L/Cr).

## Deploy workflow
Edit `index.html` locally → re-drag onto app.netlify.com (or via GitHub auto-deploy once connected) → reload live URL. Netlify serves the uploaded version, not your local copy.

## Open TODOs
- [ ] Add security: passphrase gate + Firebase Anonymous Auth + Firestore rules (currently open test mode).
- [ ] (Optional) Connect Netlify to GitHub for auto-deploy on push.
- [ ] Confirm Firebase config is pasted in and dot shows green "Synced".

## Notes / gotchas
- Data lives in BOTH Firebase (if synced) and browser localStorage. If red dot, localStorage is per-browser — use Export/Import Backup to move data between machines.
- Backup JSON files contain real financial data — they are gitignored, do NOT commit them.
- Excluded by design: gold, goals tracker, transaction-level expense tracking.
