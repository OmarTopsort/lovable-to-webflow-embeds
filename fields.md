# CMS bindings for Press Room and Company News

This reference matches `scripts/embed-src/resources-v3/cms-reader.js`. Keep existing collections and records; bind their fields to these elements instead of renaming collection IDs or rewriting published items. Read [installation.text](installation.text) for the overall update order.

## Placement and list structure

Place the hidden CMS lists before the page's JavaScript Embed parts. They must be present in the DOM when initialization runs. Use Webflow's `display: none` on a parent wrapper; do not remove the elements or hide them using a condition that omits the entire list. The reader reads only rendered items: set the intended item limit and handle collection pagination so the page receives every record it needs. A Webflow pagination button that appends data after initialization does not automatically rebuild the embed's search index.

The exact selectors are:

| Records | Required rendered selector | Used on |
|---|---|---|
| Customers | `.cms-customers .cms-item` | Press Room |
| Opinions | `.cms-opinions > [role="listitem"]` | Press Room; relevant news/update records on Company News |
| Product updates | `.cms-updates > [role="listitem"]` | Company News; searchable records on Press Room |
| Press coverage | `.cms-press > [role="listitem"]` | Press Room |
| Research | `.cms-research > [role="listitem"]` | Press Room search/filter |
| News | `.cms-news > [role="listitem"]` or `.cms-news .cms-item` | Company News |
| Videos | `.cms-videos > [role="listitem"]` or `.cms-videos .cms-item` | Reader supports them; current Press Room renderer does not consume these lists |
| Events | `.cms-events > [role="listitem"]` or `.cms-events .cms-item` | Reader supports them; Press Room currently uses the event cards shipped in the embed |

For the `>` selectors, put the class on the **Collection List element whose direct children are the Collection Items**. Putting the class only on an outer Collection List Wrapper, with a `.w-dyn-items` element between it and the list items, does not match. Set each Collection Item's `role` attribute to `listitem` when not already present. Customers need a `.cms-item` class on each item; their selector permits nesting.

Example for updates (replace sample values with CMS bindings):

```html
<div style="display:none" aria-hidden="true">
  <div class="cms-updates">
    <div role="listitem">
      <span class="cms-field-title">Actual published announcement title</span>
      <span class="cms-field-date">2026-10-07</span>
      <span class="cms-field-summary">Actual announcement summary</span>
      <span class="cms-field-card-tag">Product Updates</span>
      <a class="cms-field-link" href="/post/actual-published-slug">Read announcement</a>
    </div>
  </div>
</div>
```

Example for customers:

```html
<div class="cms-customers" style="display:none" aria-hidden="true">
  <div class="cms-item">
    <span class="cms-field-title">Actual case-study title</span>
    <span class="cms-field-company">Company name</span>
    <span class="cms-field-industry">Retail</span>
    <span class="cms-field-region">LATAM</span>
    <span class="cms-field-metric">Actual published metric</span>
    <span class="cms-field-metric-label">Metric description</span>
    <img class="cms-field-logo" src="https://your-existing-cdn/logo.svg" alt="Company name">
    <a class="cms-field-link" href="/casestudy/actual-published-slug">Read case study</a>
  </div>
</div>
```

## Field classes the reader actually supports

| Field class | Element/binding | Requirement and effect |
|---|---|---|
| `cms-field-title` | Text | Required for every record; blank titles are discarded. Use this for events too, rather than an old `cms-field-name` convention. |
| `cms-field-link` | Anchor with a real `href` | Required for a usable record. Use the CMS item URL or actual external article. Press Room filters out missing/invalid links; Company News expects valid links. No `#` placeholders. |
| `cms-field-summary` | Text | Optional summary. |
| `cms-field-date` | Text/date formatted as an unambiguous date with a year | Needed for reliable newest-first ordering; ISO `YYYY-MM-DD` is suitable. Verify ambiguous/missing dates before publishing. |
| `cms-field-card-tag` | Text | Optional display label; otherwise uses the first tag or the collection category. A trailing `*` marks the record as featured internally and is removed from display. |
| `cms-field-tag` | One or more text elements | Categories/tags. All matching tag elements are read, including nested tag/reference-list elements. |
| `cms-field-company` | Text | Company display/name matching on customer cards. |
| `cms-field-industry` | Text | Customer industry filtering; keep labels consistent. |
| `cms-field-region` | Text | Optional customer metadata. |
| `cms-field-metric` | Text | Published customer metric value. |
| `cms-field-metric-label` | Text | Metric label; note the hyphen, not `metricLabel`. |
| `cms-field-logo` | Class directly on an `img`; bind `src` | Optional company logo. The reader uses `img.cms-field-logo`, not a wrapper or CSS background. |
| `cms-field-thumbnail` | Class directly on an `img`; bind `src` | Optional record thumbnail. Customer-card design still prioritizes logos/name on gradients. |

Elements marked `w-dyn-bind-empty` are ignored. Link URLs may be root-relative or HTTPS; keep external URLs complete. Do not invent missing metrics, thumbnails or records.

Old documentation mentioned fields such as `cms-field-video-url`, `cms-field-download`, `cms-field-name`, `cms-field-outlet`, `cms-field-version`, `cms-field-authors`, and a `.cms-featured` collection. This reader does not consume those fields or that collection. Keep existing CMS schema if used elsewhere, but bind this reader's common `title`, `link`, `summary`, `date`, etc. rather than expecting unsupported fields to drive these embeds.

## Content classification

- Each collection supplies its default category.
- An Opinion record with any tag equal to `Product Updates`, `New Feature`, `Product Deep Dive`, or `Integration` (case-insensitive) becomes a Product Update.
- An Opinion title matching `OpenAI Ads`, `partnership`, or `partners with` becomes News. This is existing code behavior; review actual records affected by it.
- Explicit News records should use `.cms-news`; do not depend on title heuristics or assume a `News` tag alone changes an Opinion's category.
- Product Updates sort newest first; records without parseable dates fall after dated updates in the shared reader. Company News additionally sorts the combined News/Updates list, so give all its intended records valid dates.

## Press Room `/resources`

Keep the current customer, opinion, update, research and press lists. The renderer uses its shipped published-content snapshot when a category has no loaded CMS records; supplying a category makes those live records the category's source. It normalizes URLs and deduplicates by category plus title. The source snapshot is a fallback, not an instruction to replace the CMS database.

Customer cards show a logo or company name on a soft gradient. Recognized brands may use the bundled brand catalog. Ambev uses its name when no logo is available. Do not replace logos with old story photographs merely because thumbnails remain in the CMS.

The old standalone News section is gone. News and Product Updates are presented on Company News; do not paste an old News block back into Press Room. Search suggestions are generated from loaded post titles. Verify every visible suggestion returns a real result.

## Company News `/company/news`

Render `.cms-news`, `.cms-updates`, and/or relevant `.cms-opinions` lists before its JS. The page consumes only records classified as News or Product Updates.

When any matching live CMS rows exist, they replace the entire fallback list. Provide the full intended set of news and update rows; a one-item test list will replace the fallback with just that item. Remove test records and check counts, dates, titles, URLs and newest-first ordering before publication.

No dedicated collection is mandatory: existing collection lists with the required bindings are sufficient. Do not bulk-copy or duplicate existing items merely to satisfy the page layout.
