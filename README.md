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

## Tools (11)

| Tool | What it does |
|---|---|
| `is_hiring` | WHEN an agent already holds a company and needs to qualify it -- the cheap gate before spending a richer call. ATS/board-only and freshness-floored. Takes the integer `company_id` … |
| `get_open_reqs` | The company's current active reqs, deduped across boards -- ATS/board-only, freshness-floored. `function` must be a function id (as seen in prior results); a plain role name like … |
| `hiring_pulse` | A company's hiring velocity/direction/momentum in one call -- dual-metered via `max_age` (seconds). Cold returns `{job_id, status: "crawling"}` instead of a synchronous body, same … |
| `who_is_hiring_for` | WHEN an agent needs to FIND the companies worth working -- the sourcing step, before it knows which companies exist. Reverse who's-hiring-for {role, geo} search: companies with … |
| `search_jobs` | Flat, role-granular job search -- the individual open roles across companies matching `role` (function) / `geo` (country) / `since`, one row per logical req (each with its own … |
| `get_role` | One open role's detail, addressed by the `company_id` + `req_key` pair every `search_jobs` row already carries -- the follow-up call for a role you hold an identifier for, instead … |
| `pre_action_brief` | The motion atoms + history primitives pre-joined into one bounded, compact call -- dual-metered via `max_age`. Cold returns `{job_id, status: "crawling"}`, same as the REST route. |
| `get_changes` | The `since=cursor` diff feed -- ledger events with `event_seq > since`, plus a `next_cursor`. Meters one `change` unit per event returned, independent of poll count. A free-tier … |
| `register_webhook` | Register the delivery target a watch fires to, and get back the `webhook_endpoint_id` `watch_company` needs -- call this first if you do not already hold one. Idempotent: … |
| `watch_company` | Subscribe to a company's hiring events on a registered webhook -- `webhook_endpoint_id` must belong to the same customer as the authenticated key. Meters one `watch` unit. A free … |
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
