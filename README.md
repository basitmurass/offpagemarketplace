# OffPage Atlas — Backlink & Guest Post Marketplace

A single-file, client-side marketplace web app for selling guest posts and backlinks.
Live-synced from the owner's Google Sheet — no backend, no database, no rebuild needed for data updates.

**Current version:** v3.35
**Live site:** https://offpagemarketplace.netlify.app/ / https://offpagemarketplace.vercel.app/

---

## Features

### Marketplace (public)
- Browse/search guest-post sites and backlink services (website, niche, keywords)
- Filters: niche category, country, service type, price range, DA/DR/traffic
- 48 sites per page, 250ms debounced search
- Cart with LB- order IDs, WhatsApp order confirmation
- JazzCash / Easypaisa payment boxes
- Dark mode (deep-slate) with sun/moon toggle, 3D buttons, back-to-top

### Admin (`#murass`)
- Dashboard: sites, orders, suppliers at a glance
- Sheet editor: multi-select delete/edit, bulk price paste
- Bulk upload (CSV paste with auto-detect columns)
- Suppliers table: full-row edit (rename updates all sites/orders), WhatsApp per supplier
- Categories manager, Trash, Orders with bell + beep notification
- File uploads on cart items (names stored locally)

### Live Google Sheet sync (v3.27+)
- Paste a published CSV link in **Settings → Live Google Sheet (CSV link)** → **Save**
- App auto-syncs on save — old data replaced, new data loaded, no redeploy
- **🔍 Link test karo** button verifies the link *before* saving
- Smart link fixer: `pubhtml` links auto-convert to CSV format; edit-links rejected with a clear message
- Supplier-private overlay: vendor name / WhatsApp / cost / prices are saved in the browser
  before sync and re-applied after — supplier data never leaves the browser
- Clients always see fresh sheet data on every load

### Auto niche detection (v3.28+)
- Guest posts auto-categorized: Health, Casino, Pet, Tech, Finance, Travel, Food, Fashion,
  Sports, Education, News, Business, Real Estate, Automotive, Home
- Backlinks auto-typed: .edu, Forum, Profile, Directory, Image, Comment, Social Bookmarking,
  Web 2.0, QnA, Nofollow
- **🏷 Fix categories** button fixes only General/blank sites

---

## Deploy

Single `index.html` — drag & drop the ZIP contents onto Netlify (or any static host).

```
offpage-atlas-v3.35-netlify.zip
└── index.html
```

No build step. No environment variables.

---

## Live Sheet setup (owner)

1. In Google Sheets: **File → Share → Publish to web** → choose the `Sites` tab → **Publish**
2. Copy the link (it looks like `.../pubhtml`)
3. In the app: `#murass` → login → **Settings** → paste into **Live Google Sheet (CSV link)**
4. Click **🔍 Link test karo** — you should see `✅ Link OK — N rows milin`
5. Click **Save** — the app syncs automatically

> The app converts `pubhtml` links to CSV format by itself. Never paste the `/edit` URL.

**Sheet columns recognized:** Website, Type, Price PKR, USD, DA, DR, Spam %, Traffic,
Country, Category, TAT, Tags, Samples, Remarks, LinkType (+ any custom columns).

**Do NOT put in the sheet:** supplier names, phone numbers, cost prices — keep those in the
app only (they stay in your browser).

---

## Admin access

- URL hash: `#murass` → email + password gate
- Session is tab-scoped (`sessionStorage`), no remember-me

---

## Data model (localStorage)

| Key | Contents |
|---|---|
| `sites` | Site listings (public fields) |
| `priv` | Private supplier overlay, keyed by normalized domain |
| `suppliers` | Saved supplier list (name + WhatsApp) |
| `orders` / `cart` | Orders and cart |
| `settings` | WhatsApp, USD rate, live-sheet link, footer, payment numbers |

---

## Version history

| Ver | Notes |
|---|---|
| v3.34 | IndexedDB storage (GBs) — sites bypass localStorage 5MB limit; auto-migration of old data |
| v3.35 | New live-sheet link baked as default; 1,082 phantom baked sites removed; old dead link auto-migrates; "Live data band karo" admin switch (empty marketplace when off); paste new link in Settings → auto-sync keeps it updating |
| v3.33 | Smart compaction — strips empty fields before save (7.3MB → 3.9MB); quota fallback |
| v3.32 | Professional polish: card hover lift, gradient buttons, focus rings, smooth animations, custom scrollbar, shimmer loading, `content-visibility` fast rendering |
| v3.31 | Removed pre-live restore; smart link normalizer + test button; `normWeb` supplier-safe sync |
| v3.30 | HamaraMultan-style footer (uppercase headings, brand block, bordered social icons) |
| v3.29 | Guest Post vs Backlink category separation (`detectBacklinkType`) |
| v3.28 | Auto niche detection + Fix categories; dashboard hides zero-count presets |
| v3.27 | Settings link Save triggers automatic live sync |

---

## Notes

- Baked-in seed data lets first-time visitors see listings instantly; live sync replaces it.
- If a deploy shows stale data, check the footer version badge and hard-refresh.
- Changing prices in bulk: Sheet editor → bulk price paste, or update the Google Sheet directly.
- Storage: site listings live in IndexedDB (GBs); settings stay in localStorage.
