# neuroticsasquat.ch

Single-page landing for `neuroticsasquat.ch` — a "label" presenting a short
catalog of projects in a Discogs-inspired layout.

## Stack

Static HTML/CSS, zero build step. Deployed to Cloudflare Pages on push
to `main`.

## Local preview

Open `index.html` in a browser, or:

```bash
python -m http.server 8000
# then visit http://localhost:8000
```

## Structure

```
.
├── index.html      # the page (logo SVG is inlined here)
├── styles.css      # all styles (light + dark)
├── _headers        # Cloudflare Pages security headers + cache rules
├── img/
│   ├── sasquatch.svg    # standalone logo copy
│   ├── nsq-001.webp     # release covers (you provide)
│   ├── nsq-002.webp
│   ├── nsq-003.webp
│   └── nsq-004.webp
└── README.md
```

## Adding release cover screenshots

Each release card uses a square image as its "album cover" with the catalog
number and title overlaid on gradient scrims.

**Spec:**
- Filename: `img/nsq-NNN.webp` matching the catalog number
- Format: WebP preferred (smaller); PNG and JPG also work
- Dimensions: square, 800×800 minimum (displayed up to ~360px wide)
- The top ~35% and bottom ~55% get dark gradient scrims for text legibility,
  so the most important details of the screenshot should sit in the middle band

If an image is missing, the card falls back to a solid color field with the
catalog number and title — the layout doesn't break.

## Adding a release

In `index.html`, copy any `<article class="release">` block and edit:
- Catalog number (`NSQ-NNN`) in two places
- Title, year, blurb, format, label
- Link href and `aria-label`
- Update the `<img src="img/nsq-NNN.webp">` to match
- Pick one of the fallback color classes on the `.release-art` element:
  `art-coral`, `art-teal`, `art-amber`, `art-ink`

## Logo

The sasquatch SVG is **inlined** in `index.html` so it can inherit
`currentColor` from CSS and flip between light and dark mode. A copy lives
at `img/sasquatch.svg` for use as a favicon, OG image, etc.

## Deploy

1. Push this repo to GitHub.
2. In the Cloudflare dashboard, go to **Workers & Pages** → **Create** →
   **Pages** → **Connect to Git**, and select this repo.
3. Build settings: leave **Build command** empty, **Build output directory**
   set to `/` (or leave blank). Cloudflare just serves the repo's files.
4. Deploy. Subsequent pushes to `main` auto-deploy.
5. In the project's **Custom domains** tab, add `neuroticsasquat.ch`.
   If the domain's DNS is already on Cloudflare, this is one click; otherwise
   follow the CNAME instructions for your DNS provider.

**Free tier covers everything this site needs:** unlimited bandwidth,
500 builds/month, custom domain, automatic HTTPS.

### Headers and caching

The `_headers` file sets security headers (`X-Content-Type-Options`,
`X-Frame-Options`, `Referrer-Policy`, `Permissions-Policy`) and cache
rules. Cloudflare Pages reads this file natively — no further config needed.
