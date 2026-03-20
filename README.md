# 🔍 PriceScout

> Competitor price intelligence for your lingerie & underwear catalogue — powered by Claude AI with live web search.

**7,388 products · 72 brands · 27 categories · zero dependencies**

---

## What it does

PriceScout is a single `pricescout.html` file with your entire product catalogue embedded. Pick any product, hit **Search Competitor Prices**, and the app queries Allegro, Google Shopping, and the open web via Claude AI — returning live prices, seller names, direct links, and a comparison against your cost price.

No server. No build step. No install. Just open the file in a browser.

---

## Features

- **Product browser** — search by name, brand, EAN, type or category; filter by brand (72), category (27), and price range
- **Live competitor search** — searches Allegro.pl, Google Shopping, and general web simultaneously
- **Price comparison** — color-coded badges: 🟢 cheaper / 🟡 similar (±5%) / 🔴 dearer than your cost price
- **PLN ↔ EUR conversion** — Polish Zloty prices automatically converted for comparison
- **Product images** — loaded live from the Matterhorn wholesale CDN
- **Toggle sources** — enable/disable Allegro, Google Shopping, or web search per query

---

## Quick Start

1. Download `pricescout.html`
2. Open it in Chrome, Firefox, Edge, or Safari
3. Search or browse to a product in the left panel
4. Click a product → select your target platforms → click **Search Competitor Prices**
5. Results appear in ~5–15 seconds

> **Note:** The app calls the Anthropic API from your browser. It works out of the box when opened from within a Claude.ai session. For standalone use, see [API Key Setup](#api-key-setup) below.

---

## Hosting

It's a static file — any static host works.

| Platform | Setup | Cost | How |
|---|---|---|---|
| **Netlify Drop** | < 1 min | Free | Drag `pricescout.html` onto [app.netlify.com/drop](https://app.netlify.com/drop) |
| **GitHub Pages** | 5 min | Free | Upload file → Settings → Pages → select branch |
| **Cloudflare Pages** | 5 min | Free | Connect repo or drag & drop |
| **Vercel** | 5 min | Free | `vercel --prod` or drag & drop |
| **Any web server** | 10 min | Varies | Copy file to your server's public directory |

### GitHub Pages step-by-step

```bash
# 1. Create a repo and push the file
git init
git add pricescout.html
git commit -m "Add PriceScout"
gh repo create pricescout --public --push

# 2. Enable Pages in repo Settings → Pages → Branch: main → Save
# 3. Live at: https://<your-username>.github.io/pricescout/pricescout.html
```

---

## API Key Setup

The app calls `https://api.anthropic.com/v1/messages` directly from the browser.

### Option A — Add your key directly (internal/local use only)

Find the `fetch(` call in the file and add your key to the headers:

```js
headers: {
  "Content-Type": "application/json",
  "x-api-key": "sk-ant-YOUR_KEY_HERE",
  "anthropic-version": "2023-06-01",
  "anthropic-dangerous-direct-browser-access": "true",
},
```

⚠️ Only do this if the URL is private. Never expose your API key publicly.

### Option B — Per-user key input (recommended for teams)

Ask Claude to add a settings panel to the app where each user pastes their own key. It stays in memory only — nothing is persisted.

### Option C — Backend proxy (recommended for production)

Deploy a small proxy (Cloudflare Worker, Node.js, Python/FastAPI) that holds your key server-side. The HTML calls your proxy instead of Anthropic directly.

---

## Product Data

All 7,388 products are embedded as a JavaScript array in the HTML file. Each record:

| Field | Description |
|---|---|
| `id` | Internal product ID |
| `name` | Full product name (brand + type + ID) |
| `brand` | Brand name (e.g. Axami, Pure Sin, Atlantic) |
| `price` | Cost price in EUR |
| `category` | Product category (27 total) |
| `type` | Product type (e.g. Bra, Panties, Corset) |
| `ean` | Primary EAN barcode — used in search queries |
| `image` | Product image URL (Matterhorn wholesale CDN) |

### Updating the catalogue

Re-generate the file with a new `products.xml` by running the build script with Claude, or manually replace the `const PRODUCTS = [...]` array in the HTML source.

---

## Technical Details

| Property | Value |
|---|---|
| File type | Single-file HTML — no frameworks, no build step |
| File size | ~1.6 MB (includes all product data) |
| AI model | `claude-sonnet-4-20250514` with `web_search` tool |
| Fonts | Syne + Space Mono (Google Fonts) |
| Browser support | Chrome 90+, Firefox 88+, Edge 90+, Safari 14+ |
| PLN → EUR rate | 1 EUR ≈ 4.35 PLN (hardcoded — update as needed) |
| Max sidebar results | 200 (use filters to narrow down) |

---

## Known Limitations

- Search accuracy depends on Claude finding real listings — niche products may return fewer results
- The PLN/EUR rate is hardcoded; update `const PLN_EUR = 0.23` if the rate shifts significantly
- Product images load from Matterhorn's CDN and may fail if the CDN is unavailable
- API calls are not cached — searching the same product twice makes two separate API calls
- Sidebar shows max 200 products at a time; refine filters to find specific items

---

## Customisation

All changes can be made directly to `pricescout.html`:

- **Change PLN/EUR rate** — search for `PLN_EUR` and update the value
- **Add more platforms** — extend the `search-targets` grid and update the prompt in `runSearch()`
- **Export to CSV** — add a download button in `renderResults()`
- **Translate to Polish** — replace English UI strings throughout
- **Add price history** — store results in `localStorage` and chart them over time
- **Add API key input** — add a settings modal so the app works independently of Claude.ai

---

## Stack

Built entirely with vanilla HTML, CSS, and JavaScript — no npm, no bundler, no framework. The AI layer is a single `fetch()` call to the Anthropic Messages API with the `web_search_20250305` tool enabled.

---

*Generated with [Claude](https://claude.ai) · March 2026*
