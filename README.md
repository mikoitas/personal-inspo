# Personal inspo — site

The production static front-end. Dark, Linear-style, Library layout. It reads its content from `data/index.json`; the HTML/CSS/JS never need to change when screenshots are added or removed — only the data does.

## Structure

```
site/
  index.html        the app (HTML + CSS + JS in one file)
  robots.txt        blocks search indexing (privacy guardrail)
  data/
    index.json      the content — one object per screenshot (source of truth for the site)
    index.js        identical data as window.__INSPO__ (fallback for opening via file://)
    images/         optimized screenshots, <id>.jpg
```

## Data schema (`data/index.json`)

An array of objects:

```json
{
  "id": "01",
  "image": "data/images/01.jpg",
  "title": "Linear — product development system",
  "source_url": "https://linear.app",
  "category": "Landing Pages",
  "caption": "Dark hero for Linear with product UI preview",
  "keywords": ["linear", "hero", "dark", "saas"],
  "text": "",
  "created_at": "2026-06-13T14:07:03:00",
  "date": "Jun 13, 2026"
}
```

`category` must be one of the 10: UI, UX, Typography, Colours, Animation, Design Systems, Icons, Landing Pages, Prototypes, Illustrations. Search matches `title` + `caption` + `keywords` + `text`.

## How the pipeline will write this

When the Mac sync agent (next build step) processes a new screenshot, it will: optimize the image into `data/images/<id>.jpg`, run Claude vision to produce `category` + `caption` + `keywords` + `text`, read the source URL/title from the capture sidecar, append the record to `data/index.json` (and mirror to `index.js`), then commit + push. Deleting a screenshot removes its image + record.

## Preview locally

Opening `index.html` directly works (it falls back to `data/index.js`). For the real `fetch('data/index.json')` path, serve it:

```
cd site
python3 -m http.server 8000
# open http://localhost:8000
```

## Notes / to do before going live

- Category 3D icons currently load from the Thiings CDN at runtime. For production, download the 11 icons (free for personal use) into `data/icons/` and reference them locally so they can't break.
- Tailwind + Lucide load from CDN; fine for a static host. Could be vendored later.
- Repo should be **private**, deploying only the built site (privacy guardrail), with `robots.txt` + `noindex` already in place.
# personal-inspo
