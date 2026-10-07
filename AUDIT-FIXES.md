# Website audit fixes — September 23, 2026

These changes are local. Install the updated code in Webflow and publish to update topsort.com.

## Updated embeds

| File | Change |
| --- | --- |
| `resources.html` / `resources-v3.html` | Sort Product Updates newest first, including search results and fallback content. Recognize product-update tags anywhere in an article's CMS tags. Add a visible, associated email label and email autocomplete. Use whichever resources variant is currently installed; do not install both together. |
| `resources-content.html` | Change “whitelabelling” to “white-labeling”; give 13 customer logos company-specific descriptions; explicitly hide CMS data collections so their source links are not exposed to screen readers. If Webflow generates these collections, apply the spelling/logo corrections to the corresponding CMS fields and keep their wrappers hidden. |
| `new-form.html` | Add a visible email label and field name while preserving the responsive form layout. |
| `fall-update-2026.html` | Add spaces between the date/title spans and “Who's in the room.” Keep days and agenda text fully visible when reduced motion is enabled. |
| `global-head.html` | Remove developer comments. CSS declarations are unchanged. Replace the corresponding shared CSS block in Webflow's Head Code; preserve any other site integrations or metadata. |

The resources reader, behavior, styles, body source, and generator were updated alongside the shipped HTML, so these changes survive a resources rebuild. Dates that cannot be parsed are placed after dated updates; equal dates keep their original order. Other categories retain their editorial order.

The local product URLs already use the corrected destinations. The current affected product/solution embeds use demo links instead of the reported unlabeled email-capture forms. The reported resources headings and dead `/try` link are absent from the current resources version. Publish the current local versions to replace older installed code where applicable.

## Changes requiring Webflow or external account access

- Correct page-specific hreflang URLs using actual localized equivalents; do not advertise translations that do not exist. Set Portuguese and Spanish canonicals to their own URLs.
- Translate Spanish page content, CTAs, footer, image descriptions, cookie UI, and SEO/social/JSON-LD text. Correct Portuguese JSON-LD to describe the Portuguese page. If Spanish publication is deferred, manage its indexing in Webflow.
- Set a descriptive Resources page title and configure Open Graph/Twitter preview images in the page/head settings.
- Update shared footer story/news destinations, reconcile locale labels, add copyright text, and name GitHub, YouTube, and LinkedIn icon links. The current resources embed provides a `#press` section for a news/press destination.
- Give the shared image-only promo link an accessible name, such as “Topsort Fall Update 2026 — Boston.” Its shared markup is not in this directory.
- Review redirect rules and the 404 template, correct `/pt/start`, resolve `/contact` and remaining missing pages, and repair the CMS case-study slug with a space while preserving the appropriate redirect.
- Fix remaining placeholder anchors on Webflow-owned `/ad-network` and `/use-case/super-apps` pages.
- Remove or correctly configure ShareThis where it is installed in Webflow or a tag manager. No ShareThis loader exists in these local embeds.
- Check the existing Fall Update Vimeo player permissions/availability. Its player request returned HTTP 401 during preview testing; the video URL was preserved rather than replaced with unrelated media.

## Verification

Run from the project root:

```sh
node scripts/test-embed-audit.mjs
node scripts/test-embed-audit-browser.mjs
```

The static check covers 64 active HTML files, CSS/JavaScript syntax, reported dead destinations, accessible link/field names, heading boundaries, CMS classification, and stable date sorting. Backup/copy HTML variants are excluded.

The browser check covers both resources modes (CMS and fallback), the newsletter form, Fall Update, T-Platform, T-Engine, Tomi, Delivery Apps, and the homepage at 390, 768, and 1440 pixels. It checks overflow, email labels, runtime/console errors, update ordering, and reduced-motion readability. It does not submit forms. Fixtures, screenshots, and network findings are saved under `tmp/site-audit-fixes/browser/`.

Local previews cannot verify Webflow-generated metadata, locale routing, shared footer/promo markup, the live cookie banner, or integrations omitted from the local fixtures. Recheck those after publication.

## Shared CSS maintenance

`global-head.html` belongs before page embed styles in Webflow's site Head Code. Roots opt into the shared rules with `ts-core`, `ts-core-sol`, `ts-core-prd`, or `ts-core-cmp`; retain those classes. Developer documentation lives here and in the project guide rather than in the published head markup.
