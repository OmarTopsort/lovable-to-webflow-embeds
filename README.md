# Topsort Webflow handoff

Start with **[installation.text](installation.text)**.

The [live sitemap](https://topsort.com/sitemap.xml), fetched October 9, 2026, controls filenames and destinations. Preserve existing Webflow URLs and redirects. `/source` supplies design/content only.

- [page-map.json](page-map.json): 42 published-route files, exact destinations, existing additional URLs, paste parts, hashes and SEO.
- [sitemap-paths.json](sitemap-paths.json): verified sitemap paths.
- [seo-meta.text](seo-meta.text): SEO for the correct existing URLs.
- [fields.md](fields.md): CMS bindings.
- [_unpublished/README.md](_unpublished/README.md): three designs with no assigned live page. Do not deploy these as new routes.
- [redirects.json](redirects.json): intentionally empty; replaces the previous migration plan.

Examples: `products/t-platform.html`, `about.html`, `book-a-demo.html`, `solutions/offsite-ads.html`, and `compare/topsort-vs-cireto.html`. The comparison spelling matches the existing sitemap. Root `index.html` is `/`.

Company News lives in `resources.html` at `/resources#news`. Add `.cms-field-company-news` inside the CMS items that belong there; see `fields.md`.
