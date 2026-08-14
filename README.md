# ClinicSoftware CRM plugin

Connect [Claude Code](https://code.claude.com) to **ClinicSoftware CRM**.

## What you get

Scoped CRM tools for:

- Clients and clinic notes
- Leads / pipeline stages
- Appointments and services
- Bills and sales analytics
- Tasks
- Email / SMS (queue) and WhatsApp
- AI reply drafts

Tenant, server, staff, and salon scope come from the token — never from model-supplied IDs.

## Install

```text
/plugin marketplace add clinicsoftware/clinicsoftware-claude-plugin
/plugin install clinicsoftware@clinicsoftware-plugins
```

This plugin installs **disabled** by default (external CRM connector). Enable it:

```text
claude plugin enable clinicsoftware@clinicsoftware-plugins
```

If the install summary asks, run `/reload-plugins`, then `/clinicsoftware:setup`.

### Local test

From the marketplace root:

```bash
claude --plugin-dir ./plugins/clinicsoftware
```

## Authenticate

1. Provision a `csmcp_…` MCP token in ClinicSoftware CRM (admin).
2. Connect the `clinicsoftware` MCP server (Claude opens `/authorize`).
3. Paste the token on the authorisation page and approve.
4. Confirm in `/mcp` that the server is **connected**.

Optional terminal Bearer setup (keep the token out of chat). Prefer HTTP — Claude Code documents SSE as deprecated:

```bash
claude mcp add --transport http \
  --header "Authorization: Bearer YOUR_csmcp_TOKEN" \
  clinicsoftware https://mcp9.clinicsoftware.com/mcp
```

Legacy SSE fallback (only if the client cannot use HTTP):

```bash
claude mcp add --transport sse \
  --header "Authorization: Bearer YOUR_csmcp_TOKEN" \
  clinicsoftware https://mcp9.clinicsoftware.com/sse
```

Health check: [https://mcp9.clinicsoftware.com/healthz](https://mcp9.clinicsoftware.com/healthz)

## Example prompts

- "Search ClinicSoftware for clients named Sara"
- "List the newest 5 clients"
- "Show open leads without bookings that have treatment interest"
- "Summarise this week's sales vs last week"
- "Queue an SMS to client 12345 confirming tomorrow's appointment" (after consent checks)

## Plugin layout

```text
plugins/clinicsoftware/
├── .claude-plugin/plugin.json
├── .mcp.json
├── commands/setup.md
├── skills/setup/SKILL.md
├── skills/clinicsoftware-crm/SKILL.md
└── README.md
```

## Support

- Product: [https://clinicsoftware.com](https://clinicsoftware.com)
