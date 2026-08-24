# Inventra

A single-file, mobile-first inventory manager for small shops : track stock, keep customer credit tabs, and see your profit day by day and week by week. No install, no backend, no build step.

<!-- Optional: add a screenshot once you have one
![Inventra screenshot](screenshot.png)
-->

## Features

**Inventory**
- Add products with quantity, sale price, and profit margin (₹)
- Product-name autocomplete, learned from everything you've ever typed even after a product is removed
- Stock correction controls (+ / − / tap-to-edit exact quantity) for restocks and fixes
- A dedicated **Sell** action that reduces stock *and* logs profit, kept separate from manual corrections
- Low-stock indicator and live totals (product count, total units, low-stock count)
- Search/filter across your product list
- Two-tap confirm before deleting anything, so a stray tap can't wipe data

**Customer credit tabs**
- Add customers with an optional starting balance owed
- "Add charge" / "Record payment" buttons, or tap the balance to set it exactly
- Running total of outstanding credit across all customers
- Same autocomplete + search behavior as products

**Profit tracking**
- Automatic daily and weekly profit totals, calculated from recorded sales × margin
- This-week-vs-last-week comparison with an up/down % badge
- Day-by-day bar chart (Mon–Sun) comparing this week against last week
- Full weekly history list, most recent first

**Data & portability**
- All data persists automatically between visits, no login, no account
- Export everything to a single `.json` backup file
- Import a backup to restore data or move it to another device
- Works standalone once deployed (see [Storage](#storage) below)

**Design**
- Mobile-first layout, bottom tab navigation, thumb-sized controls
- Custom branding: favicon + "Add to Home Screen" icon (iOS/Android)
- Currency shown in ₹ with Indian number grouping
- Zero dependencies — one HTML file, inline CSS/JS

## Tech stack

Just HTML, CSS, and vanilla JavaScript in a single file (`index.html`). No frameworks, no package manager, no build step. Fonts are pulled from Google Fonts via CDN; everything else is self-contained (icons are embedded as base64 data URIs).

## Storage

Inventra needs somewhere to persist your data between visits. It auto-detects its environment:

- **Deployed anywhere else** (GitHub Pages, Netlify, your own server, or just opened locally): automatically falls back to the browser's `localStorage`.

**Important limitation:** `localStorage` is scoped to one browser on one device. If you open the deployed site on your phone and then on a laptop, they won't share data automatically. Use **Settings → Export data** to download a backup and **Import data** to load it on the other device.

## Deploying

This is a static site — one file, no build step. Any of these work:

### Netlify Drop (easiest, no account needed)
1. Go to [app.netlify.com/drop](https://app.netlify.com/drop)
2. Drag `index.html` onto the page
3. You get a live URL immediately

### GitHub Pages
1. Push this repo to GitHub
2. Go to **Settings → Pages**
3. Set source to the `main` branch, root folder
4. Your site will be live at `https://<your-username>.github.io/<repo-name>/`

### Vercel
1. Import this repo at [vercel.com/new](https://vercel.com/new)
2. No build command needed — deploy as-is
3. Get your live URL

### Cloudflare Pages
1. Connect this repo at the Cloudflare Pages dashboard
2. Framework preset: **None**, build command: (leave blank), output directory: `/`
3. Deploy

## Running locally

No server required — just open `index.html` directly in a browser. (Some browsers restrict `localStorage` for `file://` pages; if data doesn't seem to save, serve it locally instead, e.g. `python3 -m http.server` from this folder and visit `http://localhost:8000`.)

## Project structure

```
.
├── index.html   # the entire app — markup, styles, and logic
├── README.md
└── LICENSE
```

## Known limitations

- Data is per-browser/device unless you export and import a backup manually
- No multi-user sync or real-time collaboration
- No barcode scanning or camera access (browser-only, no native app APIs)
- Profit is calculated only from the dedicated "Sell" action, not from manual stock corrections

## License

MIT — see [LICENSE](LICENSE).
