# Resources Page — CMS Collection Fields

Each section maps to a Webflow CMS Collection. The JS will query hidden CMS collection list items by class name to extract field values and render the cards dynamically.

---

## How It Works

In Webflow, create a **Collection List** per section. Bind each field to a child element with the class shown below, and set the Collection List wrapper to `display: none`. The JS reads the DOM, extracts text content and `src`/`href` attributes, builds card HTML, and injects it.

**Pattern:**
```html
<!-- Hidden CMS source (rendered by Webflow) -->
<div class="cms-customers" style="display:none">
  <div class="cms-item">
    <div class="cms-field-company">Coles</div>
    <div class="cms-field-title">How Coles built a retail media engine...</div>
    <img class="cms-field-logo" src="https://..." />
    <img class="cms-field-thumbnail" src="https://..." />
    <a class="cms-field-link" href="/customers/coles"></a>
    ...
  </div>
  <div class="cms-item">...</div>
</div>
```

> **Convention for images:** Use `<img>` tags — JS reads the `src` attribute.  
> **Convention for links:** Use `<a>` tags — JS reads the `href` attribute.  
> **Convention for text:** Use `<div>` or `<span>` — JS reads `textContent`.

---

## 1. Customers

**Collection name:** `Customers`  
**Section ID:** `ts-sec-customers`  
**CMS wrapper class:** `cms-customers`

| Field | Class | Type | Required | Notes / Example |
|-------|-------|------|----------|-----------------|
| Company name | `cms-field-company` | Text | ✅ | `Coles` |
| Title | `cms-field-title` | Text | ✅ | `How Coles built a retail media engine reaching 21M weekly shoppers` |
| Metric value | `cms-field-metric` | Text | ✅ | `3.2×` |
| Metric label | `cms-field-metric-label` | Text | ✅ | `ROAS improvement` |
| Industry | `cms-field-industry` | Text | ❌ | `Food & Grocery`. Each distinct value becomes a filter pill above the customer stories; use consistent spelling |
| Region | `cms-field-region` | Text | ❌ | `APAC` — renders as tag pill |
| Logo | `cms-field-logo` | Image (`src`) | ❌ | Company logo. Falls back to text pill if missing |
| Thumbnail | `cms-field-thumbnail` | Image (`src`) | ❌ | Card hero image. Falls back to gradient bg if missing |
| Link URL | `cms-field-link` | Link (`href`) | ✅ | `/casestudy/coles`. Records without a link are not shown |

---

## 2. Opinions

**Collection name:** `Opinions`  
**Section ID:** `ts-sec-opinion`  
**CMS wrapper class:** `cms-opinions`

| Field | Class | Type | Required | Notes / Example |
|-------|-------|------|----------|-----------------|
| Tag / category | `cms-field-tag` | Text | ✅ | `Opinion`, `Perspective`, `Industry` |
| Title | `cms-field-title` | Text | ✅ | `Retail media is becoming infrastructure, not a feature` |
| Summary | `cms-field-summary` | Text | ❌ | First 1–2 sentences. Renders below title |
| Author | `cms-field-author` | Text | ❌ | `Regina Ye, CEO` |
| Date | `cms-field-date` | Text | ❌ | `Mar 2026` — displayed as-is |
| Thumbnail | `cms-field-thumbnail` | Image (`src`) | ❌ | Article hero image (not currently shown in card, but needed for og/sharing) |
| Link URL | `cms-field-link` | Link (`href`) | ❌ | `/blog/retail-media-infrastructure`. Falls back to `#` |

---

## 3. Product Updates

**Collection name:** `Product Updates`  
**Section ID:** `ts-sec-updates`  
**CMS wrapper class:** `cms-updates`

| Field | Class | Type | Required | Notes / Example |
|-------|-------|------|----------|-----------------|
| Tag / category | `cms-field-tag` | Text | ✅ | `New Feature`, `Platform`, `API`, `Launch` |
| Title | `cms-field-title` | Text | ✅ | `Tomi AI Copilot: autonomous campaign management` |
| Summary | `cms-field-summary` | Text | ❌ | Short description, 1–2 lines |
| Date | `cms-field-date` | Text | ❌ | `Mar 2026` |
| Version | `cms-field-version` | Text | ❌ | `v3.2` — renders as mono badge |
| Link URL | `cms-field-link` | Link (`href`) | ❌ | `/changelog/v3-2`. Falls back to `#` |

---

## 4. Videos

**Collection name:** `Videos`  
**Section ID:** `ts-sec-videos`  
**CMS wrapper class:** `cms-videos`

| Field | Class | Type | Required | Notes / Example |
|-------|-------|------|----------|-----------------|
| Title | `cms-field-title` | Text | ✅ | `How Topsort powers retail media at global scale` |
| Type / format | `cms-field-type` | Text | ✅ | `Explainer`, `Interview`, `Talk`, `Webinar`, `Product Demo` |
| Duration | `cms-field-duration` | Text | ✅ | `4:32` |
| Speaker | `cms-field-speaker` | Text | ❌ | `Regina Ye` |
| Company / source | `cms-field-company` | Text | ❌ | `Topsort` — shown as "Speaker · Company" |
| Thumbnail | `cms-field-thumbnail` | Image (`src`) | ❌ | Video poster image. Falls back to gradient bg |
| Video URL | `cms-field-video-url` | Link (`href`) | ❌ | YouTube/Vimeo link or embed URL |
| Link URL | `cms-field-link` | Link (`href`) | ❌ | Page URL to navigate to on click. Falls back to `#` |

---

## 5. Events

**Collection name:** `Events`  
**Section ID:** `ts-sec-events`  
**CMS wrapper class:** `cms-events`

| Field | Class | Type | Required | Notes / Example |
|-------|-------|------|----------|-----------------|
| Event name | `cms-field-name` | Text | ✅ | `Shoptalk 2026` |
| Date range | `cms-field-date` | Text | ✅ | `May 12–14, 2026` — displayed as-is |
| City | `cms-field-city` | Text | ✅ | `Las Vegas` |
| Event type | `cms-field-type` | Text | ✅ | `Conference`, `Summit`, `Expo` |
| Past event? | `cms-field-past` | Text | ❌ | Any non-empty value = "Recap". Empty = "Upcoming" |
| Link URL | `cms-field-link` | Link (`href`) | ❌ | Event page or recap URL. Falls back to `#` |

---

## 6. Press Room

**Collection name:** `Press`  
**Section ID:** `ts-sec-press`  
**CMS wrapper class:** `cms-press`

| Field | Class | Type | Required | Notes / Example |
|-------|-------|------|----------|-----------------|
| Outlet name | `cms-field-outlet` | Text | ✅ | `Forbes`, `TechCrunch` |
| Title | `cms-field-title` | Text | ✅ | `Topsort raises $20M to build the commerce-native...` |
| Date | `cms-field-date` | Text | ✅ | `Mar 15, 2026` |
| External URL | `cms-field-link` | Link (`href`) | ❌ | `https://forbes.com/...` — opens external article |

---

## 7. Research

**Collection name:** `Research`  
**Section ID:** `ts-sec-research`  
**CMS wrapper class:** `cms-research`

| Field | Class | Type | Required | Notes / Example |
|-------|-------|------|----------|-----------------|
| Title | `cms-field-title` | Text | ✅ | `A Look Forward to 2026: Retail Media Infrastructure...` |
| Paper type | `cms-field-type` | Text | ✅ | `White Paper`, `Benchmark`, `Technical Paper`, `Report` |
| Authors | `cms-field-authors` | Text | ❌ | `Topsort Research` |
| Date | `cms-field-date` | Text | ❌ | `Mar 2026` |
| Cover image | `cms-field-thumbnail` | Image (`src`) | ❌ | Cover thumbnail. Falls back to generated document placeholder |
| Download URL | `cms-field-download` | Link (`href`) | ❌ | PDF download link |
| Link URL | `cms-field-link` | Link (`href`) | ❌ | Page URL. Falls back to `#` |

---

## 8. Featured (Editor's Picks)

**Not a separate collection.** Either use a **multi-reference field** pointing to items from other collections, or add a `Featured` switch to each collection and create a filtered Collection List.

**CMS wrapper class:** `cms-featured`

| Field | Class | Type | Required | Notes / Example |
|-------|-------|------|----------|-----------------|
| Tag | `cms-field-tag` | Text | ✅ | `Case Study`, `Opinion`, `Video` |
| Title | `cms-field-title` | Text | ✅ | `How Coles built a retail media engine...` |
| Summary | `cms-field-summary` | Text | ✅ | 1–2 sentence description |
| Metric value | `cms-field-metric` | Text | ❌ | `3.2×` — shown as overlay badge on image |
| Metric label | `cms-field-metric-label` | Text | ❌ | `ROAS improvement` |
| Thumbnail | `cms-field-thumbnail` | Image (`src`) | ❌ | Hero image for featured card. Falls back to gradient + icon |
| Link URL | `cms-field-link` | Link (`href`) | ❌ | Destination page. Falls back to `#` |

---

## 9. Customer Logo Strip

Since the 2026-09-26 redesign the logo strip above the customer cards is the design's own logo artwork
(`customer-logos.svg`, about 40 customer logos), not CMS fields. Nothing to maintain in the CMS.

---

## Summary

| Collection | Text fields | Image fields | Link fields | Total |
|------------|-------------|--------------|-------------|-------|
| **Customers** | 6 | 2 (logo, thumbnail) | 1 (link) | **9** |
| **Opinions** | 5 | 1 (thumbnail) | 1 (link) | **7** |
| **Product Updates** | 5 | — | 1 (link) | **6** |
| **Videos** | 5 | 1 (thumbnail) | 2 (video-url, link) | **8** |
| **Events** | 5 | — | 1 (link) | **6** |
| **Press** | 3 | — | 1 (link) | **4** |
| **Research** | 4 | 1 (thumbnail) | 2 (download, link) | **7** |
| **Featured** | 5 | 1 (thumbnail) | 1 (link) | **7** |
