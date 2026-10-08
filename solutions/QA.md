# Solutions verification

The current source-route handoff was checked with `scripts/test-source-sync.mjs` and `scripts/test-webflow-handoff.mjs`.

- Every solution remains one file containing numbered, bounded paste parts.
- The route/file map in `../page-map.json` matches the source.
- The all-page browser check covers 1440px and 390px overflow and JavaScript errors, plus shared navigation and selected page interactions.
- Link targets may be planned source routes that are not yet published in Webflow. Use `../redirects.json` and verify the actual site after installation.

Local checks do not verify Webflow page IDs, CMS bindings, live SEO or publication. Follow `../installation.text` for those checks.
