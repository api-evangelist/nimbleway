---
name: Search the live web and extract a page
description: Use Nimble to run a live web search, then extract clean structured content from a chosen result URL.
api: openapi/nimbleway-openapi.json
operations:
  - "POST /v1/search"
  - "POST /v1/extract"
---

# Search the live web and extract a page

Base URL: `https://sdk.nimbleway.com/v1`
Auth: `Authorization: Bearer <NIMBLE_API_KEY>` on every request.

## Steps

1. **Search** — `POST /v1/search` with your query. Optionally set `country`/`locale`
   and a focus/depth mode. The response returns ranked live results with URLs.
2. **Pick a result** — choose the URL you want full content from.
3. **Extract** — `POST /v1/extract` with `{ "url": "<chosen-url>", "render": true, "country": "US" }`.
   Returns clean HTML/Markdown content for the page.
4. **For heavy pages, go async** — call `POST /v1/extract/async` to get a `task_id`,
   poll `GET /v1/tasks/{task_id}` until complete, then read `GET /v1/tasks/{task_id}/results`.

## Rules

- Requests are **not idempotent** via a client key; long jobs are tracked by the
  server-issued `task_id` (safe to re-poll).
- Respect rate limits: watch `X-RateLimit-Remaining`/`X-RateLimit-Reset`; on `429`
  back off using the `retry_after` field.
- Handle the documented error envelope `{ code, message }`: `402` = out of budget,
  `422` = invalid parameters, `429` = throttled.

See `conventions/nimbleway-conventions.yml` and `errors/nimbleway-problem-types.yml`.
