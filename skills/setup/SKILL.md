---
name: setup
description: Guide the user through connecting the ClinicSoftware CRM MCP server (OAuth with a csmcp_ token, or optional Bearer header). Use when the plugin is newly installed, /mcp shows clinicsoftware disconnected, or the user asks how to authenticate ClinicSoftware.
---

# ClinicSoftware MCP setup

## Rules

- Never ask for `server_id`, tenant/database name, or `staff_id` — the bearer token already binds them.
- Prefer browser OAuth (`/authorize`) over pasting tokens into chat.
- Never log or repeat a full `csmcp_` token.
- Prefer HTTP transport. Claude Code documents SSE as deprecated; use SSE only for legacy clients that cannot use HTTP.

## Checklist

1. Confirm the plugin is enabled (`defaultEnabled` is false — user must enable after install) and run `/reload-plugins` if needed.
2. Confirm `https://mcp9.clinicsoftware.com/healthz` is healthy when connectivity is in doubt.
3. Have the user complete OAuth with a CRM-provisioned `csmcp_` token, or use the terminal HTTP bearer path from `/clinicsoftware:setup`.
4. Verify `/mcp` lists `clinicsoftware` as connected.
5. Smoke-test with `search_clients` (omit `query`, `sort=create_date_desc`, `limit=3`) or `get_stages`.

For day-to-day CRM tool usage after auth, follow the `clinicsoftware-crm` skill.
