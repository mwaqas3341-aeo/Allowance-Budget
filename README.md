# AEO Budget Dashboard 📊

> **Automated Inspection Allowance Budget System** · Karor Male Wing
> Frontend on **GitHub Pages** · Backend on **Google Apps Script API**

---

## 🏗️ Architecture

```
GitHub Pages          Google Apps Script       Google Services
┌─────────────┐       ┌──────────────────┐     ┌──────────────┐
│  index.html │──────▶│    Code.gs       │────▶│ Google Sheets│
│  (Browser)  │ fetch │  (JSON API)      │     │ Google Docs  │
│             │◀──────│  doGet routing   │────▶│ Gmail        │
└─────────────┘  JSON └──────────────────┘     └──────────────┘
```

- **GitHub** stores and serves `index.html` via GitHub Pages
- **GAS** acts as a pure JSON API — no `google.script.run` needed
- All data calls go through `fetch()` to your GAS Web App URL

---

## 📁 Files

```
├── index.html       ← GitHub Pages frontend (Bootstrap 5, mobile-first)
├── Code.gs          ← GAS backend (JSON API via doGet)
├── appsscript.json  ← GAS manifest (timezone, webapp config)
└── README.md        ← This file
```

---

## 🚀 Setup Guide

### Part A — GitHub Pages (Frontend)

1. Push this repo to GitHub
2. Go to repo **Settings → Pages**
3. Source: `Deploy from a branch` → `main` → `/ (root)`
4. Your app is live at `https://YOUR_USERNAME.github.io/YOUR_REPO/`

---

### Part B — Google Apps Script (Backend API)

1. Go to [script.google.com](https://script.google.com) → **New project**
2. Rename the project to `AEO Budget API`
3. Replace the default `Code.gs` content with the `Code.gs` from this repo
4. Click ⚙️ **Project Settings** → enable **Show `appsscript.json` in editor**
5. Open `appsscript.json` in the editor → replace with the one from this repo
6. In `Code.gs` lines 8–9, update your IDs:

```javascript
var SHEET_ID = 'YOUR_GOOGLE_SHEET_ID_HERE';
var DOC_ID   = 'YOUR_GOOGLE_DOC_ID_HERE';
```

7. Click **Deploy → New deployment**
   - Type: **Web app**
   - Execute as: **Me**
   - Who has access: **Anyone**
8. Click **Deploy** → **Authorize** all permissions
9. Copy the **Web app URL** (looks like `https://script.google.com/macros/s/AKfyc.../exec`)

---

### Part C — Connect Frontend to Backend

1. Open your GitHub Pages URL in the browser
2. Paste the GAS Web App URL in the yellow setup box at the top
3. Click **Save** — it's stored in your browser's localStorage
4. Done! The app now talks to your GAS backend.

> The URL is saved per-browser. Each new device/browser needs this one-time paste.

---

## 📋 Google Spreadsheet Structure

### Sheet: `Raw Sheet`

| Column | Index | Field         | Notes |
|--------|-------|---------------|-------|
| B      | 1     | Personal No.  | Unique AEO ID |
| L      | 11    | Status        | Must be exactly `Active` |
| M      | 12    | Email Address | Budget email recipients |

### Sheet: `Deduction History`

| Column | Index | Field         |
|--------|-------|---------------|
| B      | 1     | Personal No.  |
| D      | 3     | Year          |
| E      | 4     | Month (name e.g. `January`) |
| F      | 5     | Deduction Amount |

---

## 📄 Google Doc Template

- Must have **one table** as the first table in the document
- Column order (0-indexed): `Sr.No | Personal No. | Name | Markaz | Tehsil | DDO | Amount`
- A footer row with no Personal No. and the word `total` in the Name cell
- Placeholder text `September 2025` somewhere in the document (gets replaced with selected period)

---

## ⚙️ Configuration

| Setting | Location | Default | Description |
|---------|----------|---------|-------------|
| Base allowance | `Code.gs` ~line 52 | `25000` | Monthly inspection allowance (PKR) |
| Date placeholder | `replaceDocDate()` | `September 2025` | Text replaced with selected period |
| Max periods | `index.html` | `4` | Max months selectable in UI |
| Drive flush delay | `Code.gs` | `2000ms` | Sleep before PDF conversion |

---

## 🔧 Troubleshooting

| Problem | Solution |
|---------|----------|
| "GAS API URL not set" | Paste your Web App URL in the yellow box and click Save |
| CORS error in browser console | Re-deploy GAS with **Anyone** access; make sure no login is required |
| Matrix loads but amounts are 0 | Check Deduction History month names match exactly (e.g. `January` not `Jan`) |
| "No valid emails found" | Column M (index 12) of Raw Sheet needs valid email addresses |
| PDF is blank | Verify Doc table has correct column structure |
| Changes not reflecting | After editing `Code.gs`, create a **New deployment** (not redeploy existing) |

---

## 📜 Changelog

### v3.0.0 (Current)
- **Architecture change**: GAS → pure JSON API, GitHub Pages hosts frontend
- Replaced `google.script.run` with `fetch()` API calls
- Added one-time API URL setup with localStorage persistence
- `doGet` router handles `preview` and `process` actions
- Fully async/await frontend with proper error handling

### v2.0.0
- Bootstrap 5 redesign, 3-step progress, mobile-first
- Refactored backend, structured JSON responses

### v1.0.0
- Initial GAS-only release

---

## 📄 License

Internal government tool — Karor Tehsil Education Department.
