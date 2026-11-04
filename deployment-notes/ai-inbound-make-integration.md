# Book a Demo — Felipe inbound AI / Make.com rollout

**Updated:** November 4, 2026. This repository's Book a Demo form is synchronized with the saved Webflow embed. Publishing Webflow is a separate action.

## Live path

`www.topsort.com/book-a-demo` browser form → **Website Leads (copy)** Make custom webhook → exclusive router:

- **Full Book a Demo submission** → persistent event-id deduplication → authenticated POST to Felipe's backend → Felix/Felipe's service handles AI qualification, HubSpot writes and assigned-rep Slack briefing. No legacy Make Slack, Sheets, Gmail or HubSpot modules run for these submissions.
- **Early Capture and other website submissions** → existing Slack → Google Sheets → Gmail → HubSpot + campaign routes. Felipe's API is not called.

**Browser webhook destination:**
`https://hook.us1.make.com/yilwrmxu7dqn2agmg5i1rrgaobp6bok4`

**Felipe API destination (Make HTTP POST only):**
`https://internal-tooling.api.topsort.ai/api/v1/webhooks/inbound`

The API bearer credential is stored in Make's `Felipe AI API Token` connection, **never** in Webflow or the GitHub repository.

## Browser form payload

For a completed form:
- `event_id` — distinct `evt_<uuid>` for this event; use for deduplication
- `submission_id` — `webflow_sub_<uuid>`, shared with early capture within the page session
- `event_type: "form_completed"`, legacy `type: "Full Form Submission"`
- `source: "topsort_website"`, `page_url`, `page`, `timestamp`
- `contact: {first_name, last_name, email, job_title, company, website}`
- `form: {message, product_interest, country}`
- Legacy top-level fields remain for existing Make mappings (`email`, `name`, `firstname`, `lastname`, `role`, `interests`, `notes`, `message`, `country`, `region`, etc.).

The form does not collect company/website or a precise job title; the corresponding canonical contact fields may be empty. Felipe's service should enrich them rather than guess submitted answers.

For early capture: `event_type: "email_captured"`, `type: "Early Capture"`; it goes through the **legacy route**, not Felipe.

Regional HubSpot meeting scheduler behavior is unchanged.

## Make.com routing & failure behavior

In `Website Leads (copy)`:
1. Route full `/book-a-demo` submissions with a present email (canonical event type or legacy full submission type) exclusively to the Felipe branch.
2. Check `Website Lead AI Events` Data Store using `event_id`, falling back to `submission_id`.
3. POST canonical JSON to Felipe, mapped from nested fields with legacy fallbacks.
4. Add Data Store record **only** after HTTP success, with `overwrite: false`.
5. On Felipe HTTP error, alert via Slack and do not mark the event processed. The tested error handler currently has no explicit `Ignore` directive.
6. Route all other email-bearing submissions exclusively to the legacy workflow.
7. Preserve Cannes, Partner Day and Developer Day campaign memberships for legacy submissions.

**Reported Make AI validation:** full demo → Felipe only; early capture → legacy only; event/newsletter → legacy only; API failure → Slack error, no dedup record; repeated event → skipped. These were scenario tests, **not proof of a live production Webflow submission**. Tests already produced real Slack/Gmail/Sheets/HubSpot side effects.

## Launch / verification

1. Activate **Website Leads (copy)** and confirm its custom webhook is listening; do not disable the old scenario until other sources using it are identified.
2. Publish the saved Webflow page to `www.topsort.com` (and `topsort.com`).
3. Submit one real test lead via the Book a Demo page and confirm one Felipe API execution, one qualification/HubSpot update, and one assigned-rep Slack briefing. Check the scheduler still appears.
4. Confirm Early Capture still enters the legacy path and doesn't trigger Felipe.
5. Check Make execution history, deduplication and error alert.
6. Retire the old scenario **only after** confirming no other website forms or integrations still target its webhook.

Note: GitHub does not deploy Webflow automatically. The Webflow site's saved third HTML embed is separately updated to the same code as this repository.
