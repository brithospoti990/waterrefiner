# WaterRefiner — Portfolio Site

Static one-page portfolio for waterrefiner.com. No build step, no dependencies — plain HTML/CSS.

## Structure

```
waterrefiner-site/
├── index.html          # the entire site
├── assets/
│   └── profile.jpg     # REPLACE with your real photo (square, ≥600×600px)
└── README.md
```

## Before deploying

1. **Replace `assets/profile.jpg`** with your real photo. Keep the filename, or update the `<img src>` in index.html.
2. Review the site descriptions and contact details in index.html.

## Deploy: GitHub + Vercel

1. Create a new GitHub repo (e.g. `waterrefiner`) and push this folder:
   ```
   git init
   git add .
   git commit -m "WaterRefiner portfolio site"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/waterrefiner.git
   git push -u origin main
   ```
2. In Vercel: **Add New → Project → Import** the repo. Framework preset: **Other**. No build command, output directory: root. Deploy.
3. Add the domain: Vercel project → **Settings → Domains → add waterrefiner.com** (and www.waterrefiner.com).
4. At your domain registrar, point DNS to Vercel:
   - `A` record for `waterrefiner.com` → `76.76.21.21`
   - `CNAME` for `www` → `cname.vercel-dns.com`
   (Vercel shows the exact records it wants on the Domains page — follow those.)
5. Wait for DNS to propagate; Vercel provisions SSL automatically.

Note: the domain currently serves an empty WordPress install. Once DNS points to Vercel, WordPress stops being served — you can cancel that hosting. The old WP install had `noindex` set; this static page has no noindex, so it will be indexable once live.

## Cloaked affiliate redirects (/info/)

The old WordPress site served 16 cloaked affiliate links at `/info/{slug}/`. These are preserved:

- `info-links.json` — source of truth (slug → destination). Edit this if a link changes.
- `vercel.json` — the actual redirects (302, both `/info/slug` and `/info/slug/` forms), generated from the manifest. If you edit a link, update BOTH files, or regenerate vercel.json.
- `robots.txt` — disallows `/info/` from crawling; vercel.json also sends `X-Robots-Tag: noindex, nofollow` on all `/info/*` responses.

**After every deploy, click-test at least these:**
- https://waterrefiner.com/info/springwell/ → SpringWell whole-house filters page with `?oid=1&affid=40` in the URL
- https://waterrefiner.com/info/springwell-ss1/ → salt softener page with `?oid=9&affid=40`
- https://waterrefiner.com/info/epic/ → Epic Water pitcher page with `?ref=PKETDGACXO`

If a click lands on the destination WITHOUT the ?oid/&affid parameters, the redirect is being stripped — check that no other rewrite rule matches first.
