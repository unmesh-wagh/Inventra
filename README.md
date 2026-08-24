# Inventra


A mobile-first inventory manager for small shops — track stock, keep customer credit tabs, see your profit day by day and week by week, and sign in to keep it all synced across your devices.

A single-file, mobile-first inventory manager for small shops : track stock, keep customer credit tabs, and see your profit day by day and week by week. No install, no backend, no build step.


<!-- Optional: add a screenshot once you have one
![Inventra screenshot](screenshot.png)
-->

**Live app:** [inventra-system.netlify.app](https://inventra-system.netlify.app/)

## Features

**Accounts & sync**
- Sign in with Google, email/password, or continue as a Guest (anonymous account)
- Once signed in, all data is stored in the cloud (Firestore) under your account — not just this device
- Open the same account on your phone and your laptop and see the same inventory, customers, and profit history
- If the network can't be reached, the app falls back to this device's local storage so it's still usable

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

**Appearance**
- Light and dark mode, toggled from Settings → Appearance, remembered on your next visit

**Data & portability**

- All data persists automatically between visits, no login, no account

- Export everything to a single `.json` backup file
- Import a backup to restore data or move it into a different account
- Works signed-in (cloud sync via Firebase) or signed-out (local-only, this device)

**Design**
- Mobile-first layout, bottom tab navigation, thumb-sized controls
- Custom branding: favicon + "Add to Home Screen" icon (iOS/Android)
- Currency shown in ₹ with Indian number grouping

## Tech stack

HTML, CSS, and vanilla JavaScript in a single file (`index.html`) — no framework, no bundler, no build step. Fonts load from Google Fonts via CDN. Authentication and cloud storage use [Firebase](https://firebase.google.com/) (Auth + Firestore), loaded via CDN as ES modules. Everything else (icons, app icon) is embedded as base64 data URIs so the file stays self-contained.

## How data storage works

- **Signed in** (Google, email/password, or Guest): every save/load goes to Firestore, scoped to your Firebase UID at `users/{uid}/appData/...`. This is what makes data follow your account across devices.
- **Firebase unreachable** (offline, blocked network, etc.): after a short timeout, the app falls back to the browser's `localStorage` so you're not stuck on the sign-in screen. That data stays local until you're back online and signed in.
- **Guest accounts** are still real Firebase accounts (with their own UID) and do sync — but only for as long as that anonymous session persists in that browser. They're not portable to a new device the way a Google or email account is, since there's no username/password to log back in with elsewhere.

## Firebase setup (for your own copy)

This repo is already wired to a Firebase project (see `firebaseConfig` inside `index.html`). If you fork this to run your own instance:

1. Create a project at [console.firebase.google.com](https://console.firebase.google.com)
2. **Build → Authentication → Sign-in method** — enable Google, Email/Password, and Anonymous
3. **Build → Firestore Database → Create database** — start in production mode
4. **Firestore → Rules** — use the rules below so people can only read/write their own data
5. **Authentication → Settings → Authorized domains** — add your deployed domain (e.g. `yourname.netlify.app`)
6. Copy your project's config (Project settings → your web app) into the `firebaseConfig` object near the top of the module script in `index.html`

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId}/appData/{docId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

## Deploying

Static site, one file, no build step — any of these work:

### Netlify (currently used for the live deploy)
Connected via GitHub → Netlify auto-deploy: pushes to `main` redeploy automatically. To set this up from scratch: [app.netlify.com](https://app.netlify.com) → "Add new site" → "Import an existing project" → pick this repo. Remember to add the resulting `*.netlify.app` domain to Firebase's Authorized domains (see setup steps above) or sign-in will fail on the live site.

### Netlify Drop (no account, no git)
Drag `index.html` onto [app.netlify.com/drop](https://app.netlify.com/drop) for an instant one-off URL. (Still needs the domain added to Firebase's Authorized domains to allow sign-in.)

### GitHub Pages
Repo Settings → Pages → deploy from `main`, root folder.

### Vercel / Cloudflare Pages
Import the repo, no build command needed, deploy as-is.

## Running locally

Just open `index.html` in a browser — no server required for the app itself. Note that `localhost` needs to be added to Firebase's Authorized domains for sign-in to work locally (Firebase usually allows `localhost` by default).

## Project structure

```
.
├── index.html   # the entire app — markup, styles, and logic
├── README.md
└── LICENSE
```

## Known limitations

- Guest (anonymous) accounts don't transfer to a new device/browser — use Google or email sign-in for that
- No multi-user collaboration on a single inventory (each account's data is private to that account)
- No barcode scanning or camera access (browser-only, no native app APIs)
- Profit is calculated only from the dedicated "Sell" action, not from manual stock corrections
- Firebase config (API key, project ID, etc.) is visible in the page source — this is normal and expected for Firebase web apps; access is actually controlled by the Firestore security rules above, not by hiding these values

## License

MIT — see [LICENSE](LICENSE).
