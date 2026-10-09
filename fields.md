# Resources CMS bindings

Keep the existing CMS collections. Place their hidden Collection Lists before the JavaScript PARTS in `resources.html`.

## Company News

- Company News is the **What's new at Topsort** section at `/resources#news`.
- Add `<div class="cms-field-company-news"></div>` inside each CMS item that belongs there. The marker may be empty; its presence is what counts.
- Omit the marker from other items. Do not merely hide it with CSS or Webflow conditional visibility: an element still present in the HTML still counts.
- Marked items appear only in Company News and search, sorted newest first. Titles, tags and the collection name do not select Company News.
- A marked item without a valid link displays as plain text. Bind a real URL to make it clickable.
- No separate Company News page or collection is needed.

```html
<div class="cms-press" style="display:none">
  <div role="listitem">
    <div class="cms-field-title">Announcement title</div>
    <div class="cms-field-date">2026-10-09</div>
    <div class="cms-field-summary">Announcement summary</div>
    <div class="cms-field-company-news"></div>
    <a class="cms-field-link" href="/post/published-slug">Read</a>
  </div>
</div>
```

## Collection selectors

| Records | Rendered selector |
|---|---|
| Customers | `.cms-customers .cms-item` |
| Opinions | `.cms-opinions > [role="listitem"]` |
| Product updates | `.cms-updates > [role="listitem"]` |
| Press coverage | `.cms-press > [role="listitem"]` |
| Research | `.cms-research > [role="listitem"]` |
| Optional existing news list | `.cms-news > [role="listitem"]` or `.cms-news .cms-item` |

Put each list class on the Collection List, not its outer wrapper. Keep all intended records rendered in the HTML; items loaded later by pagination are not automatically reread. The Company News marker works inside any supported list, including the reader's `.cms-videos` and `.cms-events` lists.

## Fields

| Class | Binding |
|---|---|
| `cms-field-title` | Required title |
| `cms-field-link` | Anchor with the real article URL; required except for plain-text Company News |
| `cms-field-company-news` | Presence marks Company News, including an empty div |
| `cms-field-summary` | Optional summary |
| `cms-field-date` | Date including year; prefer `YYYY-MM-DD` |
| `cms-field-card-tag` | Optional display label; trailing `*` marks featured content |
| `cms-field-tag` | One or more tags, including nested reference lists |
| `cms-field-company` | Customer name |
| `cms-field-industry` | Customer industry filter |
| `cms-field-region` | Optional region |
| `cms-field-metric`, `cms-field-metric-label` | Optional published customer metric and label |
| `cms-field-thumbnail`, `cms-field-logo` | Classes directly on images with bound `src` |

Unmarked Opinion items tagged Product Updates, New Feature, Product Deep Dive or Integration are categorized as Product Updates. The Company News marker takes priority. Blank titles are ignored; undated news follows dated news.

The published snapshot supplies preview content when CMS lists are absent. When any supported CMS list is present, Company News uses only its marked records; zero markers produces an empty state. Customer cards retain their logo/gradient design. Events retain the shipped event cards.
