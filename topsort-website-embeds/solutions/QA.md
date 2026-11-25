# Verification

The current routing audit checks assigned page URLs and internal links against the October 9, 2026 live sitemap snapshot. Run `node scripts/test-webflow-handoff.mjs` and `node scripts/test-embed-audit.mjs`.

Layout and interaction checks are in `scripts/test-source-sync.mjs`. Live Webflow page IDs, bindings and publication require verification in the target site, as described in `../installation.text`.
