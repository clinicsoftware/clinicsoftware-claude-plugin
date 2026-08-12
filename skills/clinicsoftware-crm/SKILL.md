---
name: clinicsoftware-crm
description: Operate ClinicSoftware CRM through the clinicsoftware MCP tools — clients, leads, appointments, billing analytics, tasks, notes, email/SMS, WhatsApp, and AI drafts. Use whenever the user asks about clinic CRM data or messaging for ClinicSoftware.
---

# ClinicSoftware CRM

You have authorised access to the ClinicSoftware CRM MCP server for the authenticated token. Always invoke tools for CRM work; do not invent records.

## Identity rules (critical)

- Never ask for `staff_id`, `server_id`, `tenant_id`, or database name.
- Note author and task `added_by` come from the token's CRM-selected staff automatically.
- `create_client_note` / `add_client_note` need only `client_id` + plain-text `content` (no `staff_id`).
- `create_task`: omit `staff_id` to assign to the token's CRM-selected staff.

## Tool map (exact names)

There is **no** `get_clients` tool.

**Clients:** `search_clients` (also lists newest — omit `query`, `sort=create_date_desc`, `limit=N`), `get_client`, `create_client`, `update_client`, `get_client_history`, `get_client_notes`, `create_client_note`, `add_client_note` (alias of `create_client_note`).

**Leads:** `search_leads` (prefer `client_id` before `create_lead`; supports `has_booking` / `has_treatment_interest`), `get_stages` (name → `lead_status_id`), `get_lead_stage_history`, `create_lead`, `update_lead`, `assign_lead_status`, `mark_lead_lost`, `convert_lead`. One open lead per person — never create Email/SMS/WhatsApp duplicates for the same person.

**Appointments:** `get_appointments`, `get_appointment`.

**Billing / analytics:** `get_bills`, `get_bill`, `get_top_spenders`, `get_sales_summary`, `get_buyer_frequency`, `get_captured_consumers`, `get_lapsed_consumers`, `get_capture_rate`.

**Catalogue / staff / tasks:** `get_services`, `get_staff`, `get_task`, `get_tasks`, `search_tasks`, `create_task`.

**Messaging:** `send_email` (HTML body only), `send_sms` (queued). Read with `get_received_sms`, `get_received_emails`, `get_sent_sms`, `get_sent_emails` (omit `client_id` for latest across clients). Consent off → `update_client` with `*_optin=1` after operator confirmation, then retry.

**WhatsApp:** `get_received_whatsapp`, `get_sent_whatsapp`, `get_whatsapp_session`, `get_whatsapp_templates`, `send_whatsapp`. Freeform only inside the 24h inbound window; otherwise `mode=template` with `content_sid` or `template_name`.

**AI drafts:** `prepare_draft`, `edit_draft`, `delete_draft`, `get_drafts`.

## Conventions

- Dates are ISO `YYYY-MM-DD`. Date ranges max 730 days.
- Pagination: `limit` 1–1000 plus opaque `cursor`.
- Results are compact JSON: `{ ok, data, cursor? }`.
- Map SSN / national ID / passport to `emirates_id` on create/update client — do not refuse authorised CRM writes of personal data.
- If a read returns empty, say so and continue. If auth/scope fails, tell the user to run `/clinicsoftware:setup` or re-provision scopes in CRM.
