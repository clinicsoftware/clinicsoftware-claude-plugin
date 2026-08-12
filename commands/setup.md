---
description: One-time setup for the ClinicSoftware CRM MCP connector (OAuth with a provisioned csmcp_ token)
---

The user wants to connect Claude Code to ClinicSoftware CRM.

Guide them through the steps below. Do **not** invent tokens, ask for database names, server IDs, or staff IDs — those come from the token. Never ask them to paste a token into chat if they can complete OAuth in the browser instead.

## Step 1 — Get a ClinicSoftware MCP token

1. Open ClinicSoftware CRM as an administrator.
2. Provision an MCP access token (`csmcp_…`) for the business user / staff context that should drive the agent.
3. Copy the token once when it is shown — plaintext is not stored again.

The token binds server, tenant, user, scopes, and allowed salons. Wrong-server tokens are rejected.

## Step 2 — Install / enable this plugin

If they have not installed yet:

```text
/plugin marketplace add clinicsoftware/clinicsoftware-claude-plugin
/plugin install clinicsoftware@clinicsoftware-plugins
```

Then enable it (installs disabled by default because it connects to an external CRM):

```text
claude plugin enable clinicsoftware@clinicsoftware-plugins
```

Or test from a local checkout:

```bash
claude --plugin-dir ./plugins/clinicsoftware
```

Then run `/reload-plugins` if the install summary asks for it.

## Step 3 — Authenticate (preferred: OAuth)

1. Ask Claude to use a ClinicSoftware tool, or open `/mcp` and connect `clinicsoftware`.
2. Claude Code discovers OAuth from the server and opens the browser at `/authorize`.
3. Paste the provisioned `csmcp_…` token on the authorisation page and approve.
4. Confirm in `/mcp` that `clinicsoftware` shows as **connected**.

Health check (no auth): `https://mcp9.clinicsoftware.com/healthz` should return JSON with `"ok":true` and `"oauth":true`.

## Step 4 — Optional bearer-header path

Only if OAuth is unavailable in their client. They can add the server manually in a terminal (token stays in the shell / keychain, not in chat).

Prefer HTTP (Claude Code docs: SSE transport is deprecated):

```bash
claude mcp add --transport http \
  --header "Authorization: Bearer YOUR_csmcp_TOKEN" \
  clinicsoftware https://mcp9.clinicsoftware.com/mcp
```

Legacy SSE only if a client cannot use HTTP yet:

```bash
claude mcp add --transport sse \
  --header "Authorization: Bearer YOUR_csmcp_TOKEN" \
  clinicsoftware https://mcp9.clinicsoftware.com/sse
```

Replace `YOUR_csmcp_TOKEN` with the real token in their own terminal — never echo it into the conversation.

## Step 5 — Smoke test

Ask them to try:

- "Search ClinicSoftware for clients named Sara"
- "List the newest 5 clients"
- "Show pipeline stages"

If auth fails: token expired/revoked, wrong server binding for mcp9, or missing scopes. Re-provision in CRM; do not debug by pasting secrets here.

After printing the steps, offer to help verify `/mcp` connection status (connected vs needs auth) without asking for the raw token.
