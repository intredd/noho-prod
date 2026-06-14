# Noho — production assets

Static artifacts for the **production** Webflow site. Source code and builds live in [intredd/noho](https://github.com/intredd/noho) (`dev` branch).

## Contents

- `dist/noho.bundle.min.js` — critical bundle (hero, preloader, Lenis, appearance, …)
- `dist/noho.bundle.deferred.min.js` — deferred bundle (product, scroll, blog, forms, …)
- `dist/noho.bundle.min.css` — Noho styles
- `dist/noho.webflow.shared.min.css` — post-processed Webflow shared CSS
- `dist/move-random-environments/` — HDR files for 3D product cards

Webflow setup: [WEBFLOW_SNIPPET.md](./WEBFLOW_SNIPPET.md).

CDN: `https://cdn.jsdelivr.net/gh/intredd/noho-prod@<SHA>/dist/...`

## Updating production

From the **dev** repo root (`noho`):

```bash
node build-local.mjs
node scripts/sync-to-prod.mjs
```

The script copies only production files into the sibling `noho-prod/` folder (or `--out=/path/to/noho-prod`).

Then in `noho-prod`:

```bash
git add -A
git commit -m "Release: <short description>"
git push
```

In Webflow Custom Code, replace `YOUR_GIT_COMMIT_SHA` with the **full commit SHA from this repo** (`noho-prod`), not from `noho`.

More detail: [docs/PROD-RELEASE.md](https://github.com/intredd/noho/blob/dev/docs/PROD-RELEASE.md) in the dev repo.
