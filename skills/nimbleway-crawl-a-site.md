---
name: Crawl a website and collect the results
description: Start a multi-page crawl with Nimble, monitor it to completion, and cancel if needed.
api: openapi/nimbleway-openapi.json
operations:
  - "POST /v1/crawl"
  - "GET /v1/crawl"
  - "GET /v1/crawl/{id}"
  - "DELETE /v1/crawl/{id}"
---

# Crawl a website and collect the results

Base URL: `https://sdk.nimbleway.com/v1`
Auth: `Authorization: Bearer <NIMBLE_API_KEY>`.

## Steps

1. **(Optional) map first** — `POST /v1/map` to discover the URL structure of the
   site via link-following and sitemaps before committing to a full crawl.
2. **Start the crawl** — `POST /v1/crawl` with the seed URL plus path filters and
   subdomain control. Returns a crawl `id`.
3. **Monitor** — `GET /v1/crawl/{id}` for status/progress; `GET /v1/crawl` lists
   your crawls.
4. **Cancel if needed** — `DELETE /v1/crawl/{id}` stops a running crawl.

## Rules

- Scope the crawl with path filters and subdomain control to stay within budget
  (`402` means insufficient budget).
- Watch rate-limit headers; back off on `429` using `retry_after`.
- Errors follow the `{ code, message }` envelope — see `errors/nimbleway-problem-types.yml`.
