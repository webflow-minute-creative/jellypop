# Jellypop — Astro migration

This project was scaffolded from a Webflow static export (`jellypop-design_webflow`).

## Getting started

```bash
npm install
npm run dev      # http://localhost:4321
npm run build    # outputs to dist/
```

## Structure

- `src/layouts/Layout.astro` — shared `<head>`/scripts pulled out of every exported
  page: meta tags, Webflow/normalize CSS, PostHog analytics, Google tag (gtag.js),
  jQuery, Webflow's runtime JS, and Splide (for auto-scrolling logo/award strips).
  Page-specific extras go through the `head-extra` and `scripts-extra` slots.
- `src/pages/index.astro` — homepage (`/`)
- `src/pages/blog.astro` — blog listing (`/blog`)
- `src/pages/style-guide.astro` — Relume style guide (`/style-guide`)
- `src/pages/404.astro` — Astro's built-in 404 page
- `src/pages/401.astro` — Webflow's password-protected-page template (see note below)
- `src/pages/collection-templates/` — Webflow **CMS Collection Page** templates
  (Blog Posts, Blog Categories, Blog Authors, FAQ). These were static templates
  bound to live CMS collections in Webflow, so the export has no real per-item
  data. Astro will still build them as real routes as-is — treat them as a
  starting point and wire them up to real content via Astro Content
  Collections, markdown, or a headless CMS.
- `public/` — images, fonts, and the original `css/`/`js/` files, copied over
  as-is and referenced with root-relative paths (e.g. `/images/...`).

## Known things to revisit

- **`/401`** — Webflow's password page posts to `/.wf_auth`, a Webflow-hosting-only
  endpoint. It won't work once hosted elsewhere; swap in your own auth if you
  still need a password gate.
- **`detail_blog-authors`** — the exported HTML for this CMS template was empty;
  you'll need to rebuild it from the live Webflow project or the Author
  collection's field structure.
- **`detail_blog-posts` JSON-LD** — the BlogPosting schema block was an empty
  placeholder in the export (Webflow fills it per-item at publish time).
- A `superflowToolbarScript` (velt.dev) tag appeared on every page in the export.
  It looked like a client-review/annotation overlay tied to the Webflow project
  ID, so it was left out of the Astro layout — add it back into `Layout.astro`
  if you want to keep using it.
- CSS/JS are unbundled, global files (matching the original export) rather than
  scoped Astro/component styles — a reasonable next step if you want to start
  breaking the page into real components (nav, footer, cards, etc.) rather than
  one big block of markup per page.
