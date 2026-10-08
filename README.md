# Topsort Webflow handoff

Start with **[installation.text](installation.text)**. It is the full implementation guide for the AI updating Webflow.

- [page-map.json](page-map.json): 46 page routes, exact embed files and paste parts, hashes, SEO, and former paths.
- [redirects.json](redirects.json): explicit URL migrations to reconcile with the existing site.
- [fields.md](fields.md): exact CMS bindings and list structure.
- [seo-meta.text](seo-meta.text): copy/paste page titles and descriptions.
- [source-pages-without-embeds.text](source-pages-without-embeds.text): source pages not yet converted. Preserve their existing content; don't create blank substitutes.

Folders mirror source **URL routes**: `platform/`, `products/`, `company/`, `solutions/`, and `compare/`. Root `index.html` is `/`; `talk-to-sales.html` is `/talk-to-sales`. T-Engine, T-Zero and Toptimize remain under `products/`, matching the source.

These are local delivery files. Webflow page creation, migration, CMS binding, redirect setup, and publication still need to be performed in the target site. Most large files contain numbered PARTS; paste each PART into its own Code Embed, in order. Do not paste an entire multi-part file into one Embed.
