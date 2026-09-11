<img src="https://reqbeat.com/favicon.svg" alt="Reqbeat" width="64" height="64">

# Reqbeat Hiring Signals

**<https://reqbeat.com/mcp/>** · [Docs](https://docs.reqbeat.com/docs) ·
[Pricing](https://reqbeat.com/pricing) · [Status](https://status.reqbeat.com/status)

Find companies hiring for a role and geo, qualify them, and watch them for changes.

Remote server, streamable-http. Nothing to run locally, nothing to build — the
endpoint below is the whole install.

## Try it in 30 seconds (no account)

```json
{
  "mcpServers": {
    "com-reqbeat-hiring-signals": {
      "url": "https://mcp.reqbeat.com/mcp"
    }
  }
}
```

Then ask your agent: *"Which companies are hiring backend engineers in the UK right now?"*

No header means demo mode: a shared credential, a smaller page and a per-IP limit. When it
runs out you get a sign-up link.

## Full access (your key)

Get a key at <https://reqbeat.com> — free tier, no card — and send it as `X-API-Key`
on the same endpoint. There is one server; the header is what picks the tier.

```json
{
  "mcpServers": {
    "com-reqbeat-hiring-signals": {
      "url": "https://mcp.reqbeat.com/mcp",
      "headers": {
        "X-API-Key": "YOUR_REQBEAT_API_KEY"
      }
    }
  }
}
```

### Claude Code

```bash
claude mcp add --transport http com-reqbeat-hiring-signals https://mcp.reqbeat.com/mcp \
  --header "X-API-Key: YOUR_REQBEAT_API_KEY"
```

### Cursor

Paste this into your browser's address bar — Cursor takes the config on the link:

```
cursor://anysphere.cursor-deeplink/mcp/install?name=com-reqbeat-hiring-signals&config=eyJ1cmwiOiJodHRwczovL21jcC5yZXFiZWF0LmNvbS9tY3AiLCJoZWFkZXJzIjp7IlgtQVBJLUtleSI6IllPVVJfUkVRQkVBVF9BUElfS0VZIn19
```

## Tools (14)

| Tool | What it does |
|---|---|
| `is_hiring` | WHEN an agent already holds a company and needs to qualify it -- the cheap gate before spending a richer call. ATS/board-only and freshness-floored. Takes the integer `company_id` … |
| `get_open_reqs` | The company's current active reqs, deduped across boards -- ATS/board-only, freshness-floored. `function` must be a function id (as seen in prior results); a plain role name like … |
| `hiring_pulse` | One number set for one company: how many reqs it opened in the last 30 days, a `velocity` ratio of that against the 30 days before it, a `direction` of up / flat / down between … |
| `who_is_hiring_for` | WHEN an agent needs to FIND the companies worth working -- the sourcing step, before it knows which companies exist. Reverse who's-hiring-for {role, geo} search: companies with … |
| `search_jobs` | Flat, role-granular job search -- the individual open roles across companies matching `role` (function) / `geo` (country) / `since`, one row per logical req (each with its own … |
| `get_role` | One open role's detail, addressed by the `company_id` + `req_key` pair every `search_jobs` row already carries -- the follow-up call for a role you hold an identifier for, instead … |
| `pre_action_brief` | Everything an agent needs before acting on one company, in one bounded round-trip instead of five: the hiring pulse, its top open reqs deduped across boards, … |
| `get_changes` | The change feed, not a search: ledger events -- a req opened, re-observed, reposted or closed -- with `event_seq > since`, ascending, plus `next_cursor`. Replay with `next_cursor` … |
| `find_company` | WHEN you hold a company's website or name but not the `company_id` every company-scoped tool takes (`is_hiring`, `get_open_reqs`, `hiring_pulse`, `pre_action_brief`, … |
| `register_webhook` | Register the delivery target a watch fires to, and get back the `webhook_endpoint_id` `watch_company` needs -- call this first if you do not already hold one. Idempotent: … |
| `watch_company` | Subscribe to a company's hiring events on a registered webhook -- `webhook_endpoint_id` must belong to the same customer as the authenticated key. Meters one `watch` unit. A free … |
| `list_watches` | WHEN you need to see what you are subscribed to -- before adding a watch, or to find the `id` `cancel_watch` takes. Returns your live watches newest first, cancelled ones … |
| `cancel_watch` | WHEN a watch should stop firing. Takes the `id` `watch_company` returned (or `list_watches` lists): {"watch_id": 42} -> {"id": 42, "canceled": true}. Idempotent -- cancelling a … |
| `write_outcome` | Write back a conversion outcome for `company_id` -- the label-flywheel substrate. Appends to `outcome_labels` scoped to the caller's own customer. Plane session only: … |

Summaries are trimmed for reading. Your client gets the full descriptions live from
`tools/list`, and this table is re-rendered from the same document.

## What it is not

A rolling 30-day window over open requisitions, not multi-year history. **No contact data
and no people** — this plane sells hiring demand, not person records.

## Links

- Registry: `com.reqbeat/hiring-signals` on the
  [official MCP registry](https://registry.modelcontextprotocol.io/v0/servers?search=reqbeat)
- [Smithery](https://smithery.ai/server/reqbeat/hiring-signals) ·
  [Cursor](https://cursor.com/marketplace/reqbeat)
- Support: <support@reqbeat.com>
- License: MIT (this repo). The hosted service is governed by
  <https://reqbeat.com/terms>.

## Generated, not hand-written

Every file here is rendered from the live `server.json` at https://mcp.reqbeat.com/server.json, and
pushed by `static-site/deploy/mcp_server_repo_publish.py` in reqbeat.git. Edit the
renderer, never these files — a hand-edit is replaced on the next publish, silently.
