# SKU Trainer

A study game for memorizing 400+ product SKUs across 3 datasets (TG/TOP,
Lider, Other), 71 categories, and 38 color variants. Built for comp-test
prep — flashcards, multiple-choice quiz, and progress tracking.

## Folder structure

```
sku-trainer/
├── index.html              ← app shell (loads everything below)
├── css/
│   └── style.css           ← all styles
├── data/
│   └── catalog.js          ← AUTO-GENERATED product data (71 categories,
│                              400 products, 38 colors, overlap map)
├── js/
│   └── app.jsx             ← React app (catalog, flashcards, quiz, stats)
├── scripts/
│   └── import_excel.py     ← Python script: Excel → catalog.js
├── vercel.json             ← Vercel deploy config (static, no build)
├── .gitignore
└── README.md
```

## How it works

- **No build step.** React + Babel load from CDN; JSX is transpiled in the
  browser. Just open `index.html` or deploy as-is.
- **`data/catalog.js`** is the single source of truth for products. It's
  plain JavaScript (no JSX), auto-generated from your Excel file. The app
  reads these globals: `CATEGORIES`, `COLORS`, `PRODUCTS`, `DATASETS`,
  `OVERLAPPING_CATEGORIES`.
- **Progress** (scores, streaks, weak-SKU list) saves in each person's
  browser via `localStorage`. Private per device; no server needed.

## Deploy to Vercel (free)

1. Create a GitHub account at github.com (free).
2. **New repository** → name it `sku-trainer` → **Create**.
3. Click **uploading an existing file** → drag in ALL the files and folders
   from this project → **Commit changes**.
4. Go to **vercel.com** → **Sign Up with GitHub** → pick **Hobby** (free).
5. **Add New → Project** → import `sku-trainer` → **Deploy**.
6. You get a link like `sku-trainer.vercel.app`. Share it. Done.

## Updating the product catalog

When the Excel source changes:

```bash
pip install openpyxl
python scripts/import_excel.py path/to/SKU_Type_Category_List.xlsx
```

This regenerates `data/catalog.js`. Commit and push; Vercel redeploys
automatically.

Or edit `data/catalog.js` by hand — it's readable JavaScript.

## Dataset rules

| Prefix     | Dataset   | Brands   |
|-----------|-----------|----------|
| T / TG    | Dataset 1 | TG, TOP  |
| L         | Dataset 2 | Lider    |
| Other     | Dataset 3 | —        |

Categories (71 total, numbered 1–71) are **global** — shared across
datasets. 21 categories overlap between Dataset 1 and 2.

## Tech stack

- **JavaScript** (React 18 via CDN) — the app
- **Python** (openpyxl) — one-time data prep from Excel
- **HTML + CSS** — no framework, no bundler, no build
- **Vercel** — free static hosting