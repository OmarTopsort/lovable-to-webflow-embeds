# Holly SEO / AEO engineering verification — 2026-11-12

**Request:** Holly's technical SEO/AEO audit sent in the Topsort Slack DM on 2026-11-09 at 22:48 EET, followed by “can we run this” at 22:48:13 and “seo and aeo should be an ongoing progress” on Nov 10.  
**Scope:** Topsort Webflow site `64761186cb9c6205851bddcb`, English-first, without adding translations or changing unrelated CRM/vendor work.  
**Release inspected:** Production HTML “Last Published: Wed Nov 11 2026 22:22:26 GMT+0000”; Webflow reports custom-domain publish on Nov 11 22:22:59 UTC.  
**GitHub repo:** `OmarTopsort/lovable-to-webflow-embeds`; SEO/CMS settings are managed in Webflow, not assumed to be stored in the embed repository.

## Ten requested checks

| # | Check | Verification status | Evidence and remaining work |
|---|---|---|---|
| 1 | Indexability, canonical, XML sitemap | **Pass for sampled priority pages; not sitewide certified** | 1,268 live sitemap URLs: 447 primary English, 410 Spanish, 411 Portuguese; /start removed. GSC inspection of home, retail-media EN/ES/PT, products/t-platform, products/t-brain, /compare, /resources and two articles returned indexed, index allowed, preferred canonical matched. Inspection reports earlier crawls, not a guarantee of Google's future behavior. |
| 2 | Server-rendered content and links | **Pass on inspected priority pages** | Home, product, solutions and article pages return 200 HTML with visible heading text, primary content, nav and internal links without needing JS execution. |
| 3 | Structured data | **Pass on representative templates** | Home: Organization + WebSite + WebPage; T-Brain/T-Platform/T-Engine: BreadcrumbList + WebPage + SoftwareApplication; article templates: BlogPosting; /compare: CollectionPage. This is not a guarantee of rich results or AI citations. |
| 4 | Product names, descriptions and factual consistency | **Partially verified** | English Webflow SEO titles/descriptions are distinct and product descriptions correspond to live page content. Sitewide product-marketing claim accuracy remains an editorial ownership matter; do not invent figures or update claims without source verification. |
| 5 | Internal links between commercial and educational pages | **Pass on inspected pages** | Earlier work linked Sponsored Listings, T-Platform and T-Brain to guides, case studies and docs. Nov 12 50-page OpenSEO crawl found no broken internal links. No exhaustive crawl of every page. |
| 6 | International hreflang, localized metadata and canonicals | **Open issue** | Genuinely translated /es/retail-media and /pt/retail-media have localized metadata and self-canonicals and were indexed in GSC. However English /compare advertises a pt-BR alternate at /pt/contato-br-copy-2, a separate Portuguese contact page, which is not a true content equivalent. Correct mapping through Webflow locale settings without deleting, translating or repurposing the contact page. Webflow connected API does not expose per-page hreflang editing; auto-generation is sitewide. Some ES/PT legacy pages inherit English titles/descriptions; user explicitly restricts unrequested translations. |
| 7 | Mobile Core Web Vitals | **Partially verified** | Prior Google PageSpeed mobile field data: LCP about 1.6–1.7 seconds, CLS 0, Core Web Vitals passed; mobile INP field value unavailable and Lighthouse lab performance around 70 on one run. Cannot claim a measured INP. |
| 8 | AI crawler access / robots / CDN | **Pass for robots, partial for CDN** | Live robots.txt only sets User-agent: * with no disallow and declares sitemap, allowing Googlebot, OAI-SearchBot and other crawlers by robots policy. CDN/bot-control policies are not fully exposed through these connectors; earlier live bot-fetch tests reported success, but never guarantee future access. |
| 9 | Authorship, dates, methodology and primary sources | **Partially verified** | Prior fixes added person schema only for explicitly credited experts and nine posts credited to Holly, organization author elsewhere; publication dates and authoritative references on selected articles. Exhaustive editorial attribution or verification of every factual claim is not established. |
| 10 | Duplicate content / old vs new product pages | **Partially verified** | Prior English cleanup excluded 41 zero-click syndicated newsroom duplicates; updated CMS metadata/H1. New 50-page audit reports 29 duplicate titles and 29 duplicate descriptions, mostly across inherited English /es and /pt legacy pages; no confirmed canonical conflicts or duplicate-content flags in that sample. Distinguish duplicate metadata from duplicate body content. |

## Continued monitoring

- OpenSEO AI visibility tracker **configured and enabled**, **weekly**, with six neutral prompts across ChatGPT, Gemini and Google AI Overview; **next scheduled check 2026-11-19 06:19 UTC**. No results/baseline recorded yet, so don't claim AI visibility changes.
- GA4 account `Topsort Website`, measurement `G-VBEMJMJE20`: connected, enhanced measurements active, health check reported zero configuration issues.
- Google Search Console connected, with 28-day query and page-level performance available; inspection of 10 requested key URLs returned **PASS**.
- Nov 12 OpenSEO crawl ID `e2e1d747-0ba1-43f8-9129-2dd0aa6d53a2` was requested at 500 pages, but completed **only 50**, with `crawlStop=crawl-page-limit`. 95 reported findings: 29 duplicate titles, 29 duplicate descriptions, 35 heading-order skips (informational), 1 slow response (informational), 1 crawl-limit warning; zero broken links, missing alts or canonical conflict flags in that sample.
- OpenSEO project `ba19ab47-686f-41f1-a879-5fb62f684b25`: [latest audit](https://app.openseo.so/p/ba19ab47-686f-41f1-a879-5fb62f684b25/audit?auditId=e2e1d747-0ba1-43f8-9129-2dd0aa6d53a2).

## Critical disambiguation: Cloudflare challenge markup is not a page's robots directive

Searches over the **entire** live page HTML falsely detected `<meta name="robots" content="noindex, nofollow">` on /es/retail-media and /pt/retail-media. Full HTML inspection found that markup **after `</head>`**, embedded as part of Cloudflare challenge content, not as a page-level robots directive. The actual page heads have no noindex tag. Both localized retail-media pages were inspected as indexed by GSC. **Do not apply a robots override to either page.**

By contrast, the untranslated /es/products/t-brain and /pt/products/t-brain variants and /es/compare and /pt/contato-br-copy-2 have genuine generated `<meta content="noindex" name="robots">` inside `<head>`, consistent with their excluded sitemap status (compare locales) or English-canonical handling (some unlocalized products).

## Remaining work / blockers

1. **Webflow localized routing (open):** Fix English `/compare` PT hreflang pointing at Portuguese contact page. Its localized Webflow page ID is `6a95d20b240016d964368594`, whose Portuguese slug is `contato-br-copy-2`. Both ES and PT variants are noindex and excluded from the generated sitemap. Do **not** change PT slug to `compare` without editorial content migration, or globally turn off generated hreflang. Connected MCP lacks a page-specific hreflang override. Correct through Webflow Designer Localize controls or an appropriately scoped future API, then republish and fetch live HTML to verify.
2. **CWV evidence (open):** Measure mobile INP when Chrome UX Report field data exists; compare against LCP/CLS. Lab tests are not field INP.
3. **Breadth (open):** Crawl/validate beyond 50 sampled URLs with a higher crawl allotment or independent sitemap-driven QA. The current OpenSEO account hit a 50-page limit despite requesting 500.
4. **Editorial optional:** Validate all source claims and authors if assigning a complete sitewide authorship/data reliability signoff; don't fabricate.
5. **Continuous tracking:** Check the first scheduled AEO run after Nov 19, 2026 and monitor Search Console and real CWV.

## Source links
- [Holly's in-scope audit request (Slack)](https://app.slack.com/client/TQ12WNEJF/D08D0MEMFFB)
- [Live Topsort sitemap](https://www.topsort.com/sitemap.xml)
- [Live robots.txt](https://www.topsort.com/robots.txt)
- [Webflow localized SEO documentation](https://help.webflow.com/hc/en-us/articles/33961235675155-Localized-SEO-and-locale-routing)
- [Webflow locale page settings](https://help.webflow.com/hc/en-us/articles/33961235760531-Localize-page-settings)

**Scope note:** The separate Capterra claim originated Nov 6 (before this Nov 9–11 task window); follow-up messages Nov 11 identify missing admin access. It is not presented as a newly requested task within the chosen 3-day window.
