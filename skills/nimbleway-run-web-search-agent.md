---
name: Run a Web Search Agent (async, with task polling)
description: Discover an existing Nimble Web Search Agent and run it asynchronously, polling the task to completion.
api: openapi/nimbleway-openapi.json
operations:
  - "GET /v1/agents"
  - "GET /v1/agents/{agent_name}"
  - "POST /v1/agents/async"
  - "GET /v1/tasks/{task_id}"
  - "GET /v1/tasks/{task_id}/results"
---

# Run a Web Search Agent (async, with task polling)

Base URL: `https://sdk.nimbleway.com/v1`
Auth: `Authorization: Bearer <NIMBLE_API_KEY>`.

## Steps

1. **List agents** — `GET /v1/agents` to find a Web Search Agent for your target
   site/domain. Inspect one with `GET /v1/agents/{agent_name}`.
2. **(Optional) check the driver** — `GET /v1/domain-knowledge/driver?agent={agent_name}`
   to see the recommended driver and any detected antibot systems before running at scale.
3. **Run async** — `POST /v1/agents/async` with the agent name and inputs.
   Returns a `task_id`.
4. **Poll** — `GET /v1/tasks/{task_id}` until status is complete.
5. **Read results** — `GET /v1/tasks/{task_id}/results`.

## Rules

- Prefer the async endpoint for large or slow targets; use `POST /v1/agents/run`
  only for quick real-time calls.
- To generate a brand-new agent, use `POST /v1/agents/generations` then poll
  `GET /v1/agents/generations/{generation_id}`.
- Rate-limit + error handling per `conventions/nimbleway-conventions.yml`.
