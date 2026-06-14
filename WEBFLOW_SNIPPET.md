# Webflow Custom Code — Noho Production

Static tags in Webflow Project Settings. No Slater, no dynamic loader.

**CDN:** [intredd/noho-prod](https://github.com/intredd/noho-prod) — use this repo only for production.  
Replace `YOUR_GIT_COMMIT_SHA` with the **full commit SHA from `noho-prod`** (not from the dev `noho` repo).

Updating artifacts: see [README.md](./README.md).

**jQuery:** Webflow always injects **jQuery 3.5.1**. Do **not** add your own jQuery `<script>` in Custom Code.

---

## Head — Project Settings → Custom Code → **Head**

```html
<!-- Noho prod: preconnect + preload + styles -->
<link rel="preconnect" href="https://cdn.jsdelivr.net" crossorigin />
<link rel="preconnect" href="https://unpkg.com" crossorigin />
<link rel="preconnect" href="https://cdn.prod.website-files.com" crossorigin />

<link
  rel="preload"
  href="https://cdn.jsdelivr.net/gh/intredd/noho-prod@YOUR_GIT_COMMIT_SHA/dist/noho.bundle.min.js"
  as="script"
/>
<link
  rel="preload"
  href="https://cdn.jsdelivr.net/gh/intredd/noho-prod@YOUR_GIT_COMMIT_SHA/dist/noho.bundle.deferred.min.js"
  as="script"
/>
<link
  rel="modulepreload"
  href="https://unpkg.com/@google/model-viewer@4.2.0/dist/model-viewer.min.js"
/>
<!-- LCP hero: replace URL after Lighthouse/trace if LCP element differs -->
<link
  rel="preload"
  as="image"
  type="image/webp"
  href="https://cdn.prod.website-files.com/667aaef1d0489a9d5183ef56/69ef4f579c7e05ad48364125_noho-hero-3.webp"
  fetchpriority="high"
/>

<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/gh/intredd/noho-prod@YOUR_GIT_COMMIT_SHA/dist/noho.bundle.min.css"
/>
<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/gh/intredd/noho-prod@YOUR_GIT_COMMIT_SHA/dist/noho.webflow.shared.min.css"
/>

<style>
  ::selection {
    background-color: black;
  }
</style>
```

---

## Footer — Project Settings → Custom Code → **Before `</body>`**

```html
<!-- Noho prod: dependencies (order matters), all deferred -->
<!-- jQuery: do not add — Webflow jQuery 3.5.1 is already on the page -->
<script defer src="https://cdn.jsdelivr.net/npm/gsap@3.15.0/dist/gsap.min.js"></script>
<script defer src="https://cdn.jsdelivr.net/npm/gsap@3.15.0/dist/ScrollTrigger.min.js"></script>
<script defer src="https://cdn.jsdelivr.net/npm/gsap@3.15.0/dist/CustomEase.min.js"></script>
<script defer src="https://cdn.jsdelivr.net/npm/gsap@3.15/dist/SplitText.min.js"></script>
<script defer src="https://cdn.jsdelivr.net/npm/lenis@1.3.23/dist/lenis.min.js"></script>
<script defer src="https://unpkg.com/embla-carousel@8.6.0/embla-carousel.umd.js"></script>
<script defer src="https://unpkg.com/embla-carousel-auto-scroll@8.6.0/embla-carousel-auto-scroll.umd.js"></script>

<!-- type=module is deferred by default; keep before Noho bundle -->
<script
  type="module"
  src="https://unpkg.com/@google/model-viewer@4.2.0/dist/model-viewer.min.js"
></script>

<!-- Noho app: critical first, deferred second (both defer, order matters) -->
<script defer src="https://cdn.jsdelivr.net/gh/intredd/noho-prod@YOUR_GIT_COMMIT_SHA/dist/noho.bundle.min.js"></script>
<script defer src="https://cdn.jsdelivr.net/gh/intredd/noho-prod@YOUR_GIT_COMMIT_SHA/dist/noho.bundle.deferred.min.js"></script>
```

---

## Notes

- **Split:** `noho.bundle.min.js` — hero, header, preloader, Lenis, appearance. `noho.bundle.deferred.min.js` — product/viewer, scroll, blog, forms. Both use `defer`; critical runs first.
- Dependency order: Webflow jQuery 3.5.1 → GSAP (+ plugins) → Lenis → Embla → `model-viewer` → Noho.
- **model-viewer** — do not lazy-load. Use `modulepreload` in Head and `type="module"` in footer.
- Embla: pin **@8.6.0** only (do not duplicate unversioned URLs).
- **HDR** move-random: 7 files in `dist/move-random-environments/`; prewarmed in the background after intro.
- Dev / Slater / Vite: [intredd/noho](https://github.com/intredd/noho), see `WEBFLOW_SNIPPET.md` there.
