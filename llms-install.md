# Installing Reqbeat Hiring Signals

Remote MCP server at `https://mcp.reqbeat.com/mcp` (streamable-http). Nothing runs
locally; there is no package to install and no process to supervise.

1. **Keyless demo (do this first).** Add the entry below under
   `mcpServers` in your client config. Sending no `X-API-Key` header is what
   selects demo mode — there is nothing else to set.

```json
{
  "mcpServers": {
    "com-reqbeat-hiring-signals": {
      "url": "https://mcp.reqbeat.com/mcp"
    }
  }
}
```

2. **Full access.** Get a key at https://reqbeat.com (free tier, no card) and add
   `"headers": {"X-API-Key": "<your key>"}` to that same entry.

3. **Verify.** Call `tools/list` — it answers with 11 tools:
   `is_hiring`, `get_open_reqs`, `hiring_pulse`, `who_is_hiring_for`, `search_jobs`, `get_role`, `pre_action_brief`, `get_changes`, `register_webhook`, `watch_company`, `write_outcome`.

## Troubleshooting

- **401** — the header is `X-API-Key`, not `Authorization`.
- **Results marked as demo** — no key was sent. Add the `X-API-Key` header.
- **A company answers "crawling"** — it has no ATS data indexed yet. Retry later; this is
  a state, not an error.

Support: support@reqbeat.com
