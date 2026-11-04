# AI Inbound Service — Make.com integration checklist

**Implementation status:** Webflow payload enhancement is proposed in this PR, not yet deployed. The existing browser -> Make webhook and HubSpot region-based scheduler stay unchanged.

## Architecture

Browser form -> existing Make webhook -> (existing Slack/CRM actions continue) + **new Make HTTP POST** -> Felipe's internal inbound API.

The bearer token MUST be stored in Make's secured HTTP/API-key credentials. **Never** put it in Webflow, browser JS, GitHub, Slack, or public docs. The token shared in chat should be rotated before going live.

## Webflow full-form event (new + legacy fields)

New canonical fields:
- `event_id`: unique `evt_<uuid>`, stable for retries **of that event**.
- `submission_id`: `webflow_sub_<uuid>`, stable between early-capture and full-submit actions in the same page session.
- `event_type`: `form_completed`
- `source`: `topsort_website`
- `page_url`: current page
- `timestamp`: ISO 8601
- `contact.first_name`, `contact.last_name`, `contact.email`
- `form.message`: raw free-text notes
- `form.product_interest`: comma-separated interest-chip labels
- `form.country`: geolocation-derived country, which may be empty
- `contact.job_title`, `contact.company`, `contact.website`: empty because the current form does not collect these details. Enrich on the server; never fabricate a submitted value.

Legacy fields `email`, `name`, `firstname`, `lastname`, `company`, `role`, `companySize`, `companyDomain`, `interests`, `notes`, `message`, `type`, `page`, `page_title`, `country`, `region`, `timestamp` remain, so existing Make mapping can continue.

Early-capture event:
- `event_type`: `email_captured` (legacy `type` = `Early Capture`)
- Includes `submission_id`, `event_id`, and `email`.
- **Do not forward to the backend as `form_completed`**. If Felipe only supports `form_completed`, skip the new HTTP module for early captures.

## Make scenario: new HTTP request

1. Leave existing Slack/CRM modules active during migration.
2. Add a router filter where `event_type = form_completed` OR legacy `type = Full Form Submission`.
3. Add Make HTTP `Make a request`: POST `https://internal-tooling.api.topsort.ai/api/v1/webhooks/inbound`.
4. `Content-Type: application/json`, `Authorization: Bearer <ROTATED_TOKEN_IN_MAKE_SECURE_CREDENTIALS>`.
5. Request JSON structure:

```json
{
  "event_id": "{{event_id}}",
  "submission_id": "{{submission_id}}",
  "event_type": "form_completed",
  "source": "topsort_website",
  "page_url": "{{page_url}}",
  "timestamp": "{{timestamp}}",
  "contact": {
    "first_name": "{{contact.first_name}}",
    "last_name": "{{contact.last_name}}",
    "email": "{{contact.email}}",
    "job_title": "{{contact.job_title}}",
    "company": "{{contact.company}}",
    "website": "{{contact.website}}"
  },
  "form": {
    "message": "{{form.message}}",
    "product_interest": "{{form.product_interest}}",
    "country": "{{form.country}}"
  }
}
```

The curly-brace placeholders are illustrative: map them using Make's variable picker. For initial testing, map to Make sample payload from a safe test submission, not real lead PII.

6. Configure retry/backoff for 429 and 5xx; use Felipe's response codes and idempotency semantics. Do not retry a 4xx validation error automatically.
7. Notify the operational owner on terminal failures. Persist request/event IDs (not tokens) for troubleshooting.
8. Do **not** run both legacy CRM writers and Felipe's new CRM writer for the same fields until ownership is defined; avoid duplicate contacts, Slack messages, and deals.
9. Test one authorized sample end-to-end, confirming the backend returned 2xx, HubSpot was enriched, one Slack alert fired, and the HubSpot scheduler still loads.

## Felipe must confirm before enabling

- Supports `event_type = form_completed` and string `event_id`/`submission_id`.
- Accepts empty optional values for job title, company, website, country.
- Supports idempotent `event_id` retries.
- Returns a clear status code on success/validation errors.
- Clarifies who writes HubSpot/Slack and expected response time.
- Token rotation and storage policy.
- CORS is not required: Make calls Felipe server-to-server.

## Rollout

1. Rotate bearer token and store only in Make.
2. Publish PR's updated embedded code to Webflow as a separate reviewed deployment; the repo commit alone does not update the live page.
3. Capture the new payload in a staging Make route.
4. Configure new authenticated HTTP POST, initially in test mode.
5. Validate success, duplicates, early-capture behavior, and scheduler.
6. Enable production routing and monitor failures.

## Blockers

- No Make.com account/scenario access through the current tools: you must make the HTTP module change.
- The actual backend endpoint has not been called from here (to avoid unauthorized test events and secret exposure).
- Webflow production publishing requires updating the page's third of six embedded HTML chunks (not just this GitHub file).
- Token currently appeared in a conversation; rotate before use.
