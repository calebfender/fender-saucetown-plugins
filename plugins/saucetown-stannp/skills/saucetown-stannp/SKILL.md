---
name: saucetown-stannp
description: Use the Sauce Town Stannp connection for reads and human-approved changes through Fender Tools.
---

# Sauce Town Stannp

Use the hosted Stannp MCP connection for connection status, account balance, bounded reporting, and supported mail proposals. Discover the current write catalog; recipient management, top-ups, SMS, and binary uploads are not exposed by this release.

## Setup

Use the plugin's hosted MCP connection. When Codex requests authorization,
open its browser sign-in flow, sign in with the email and regular password
provided by Fender, select the permitted company workspace, and approve access.
Never request a password, provider key, personal token, or OAuth credential in
chat. No helper program or operating-system installer is required.

Call `connection_status` before reporting data. Use `stannp_balance` for the
current balance and `stannp_reporting_summary` with a bounded date range.

## Recovery

- Authorization expired or revoked: reconnect using Codex's browser sign-in flow.
- Forgotten password: use Forgot password at https://portal.fenderindustries.com/login.
- Permission denied: ask Fender to adjust workspace access; reconnecting cannot grant permissions.
- Provider credential failure: ask Fender to repair the company connection.
- Provider outage or timeout: retry later without rotating credentials.

## Response handling

Provider responses retain their original data types and may be nested under
`data` and `statusCode`. Balance values may be decimal strings such as `"23.0200"`.
Validate and convert them to a finite decimal value before numeric comparisons;
use decimal-safe arithmetic for money. Do not treat a missing or invalid balance
as zero, infer a currency that was not returned, or compare money as text.
A successful empty reporting result means no matching records for the requested
period. It is not a provider failure. Keep reporting bounded and omit unrelated
personal data from summaries and logs.

## Preparing changes

1. Discover supported changes with `list_write_operations`, using search and pagination. Get the exact schema with `get_write_operation`; never invent an endpoint or input field.
2. Read the current target records and collect missing details. Explain the intended change, affected records or recipients, and financial or destructive effects.
3. Call `prepare_write` with the chosen operation ID and `{ parameters, body }` arguments. This creates a proposal; it does not change provider data.
4. Give the user the returned `approvalUrl`. Check `canApprove` and `approvalRequirement`; an administrator may first need to grant that operation under the client's write permissions at https://portal.fenderindustries.com/admin.
5. The user reviews the exact company, account and fields in the portal and confirms with their password. Never ask for that password in chat, approve through browser automation, or submit the approval endpoint on the user's behalf.
6. Use `write_action_status` after approval. Report success only for `succeeded`, and perform a narrow read-back where supported. A draft is not a completed action.

Proposals expire after 15 minutes. Changing the target or payload requires a new proposal and approval. Do not retry a `running` or `uncertain` action, including by making a new proposal; reconcile with provider records first. Provider responses may include sensitive data: summarize only what is necessary.

Each approval authorizes one exact proposal. Repeated approvals do not grant standing permission. Recurring automation execution is not available in this release. You may help design a workflow, but must not claim it is scheduled or authorized until an explicit, bounded automation policy and execution system exist. Multi-step workflows currently require approval for each mutation.

Mail proposals can incur charges and contact real recipients. Confirm the recipient, content, and live-versus-test choice. Remote artwork/PDF URLs are fetched by Stannp and may change: use stable, client-approved documents and review their contents before approval. A field preview is not a rendered proof. Test mode must be explicitly selected; never silently turn a test into live mail.
