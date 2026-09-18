---
name: saucetown-buildium
description: Use the Sauce Town Buildium connection for reads and human-approved changes through Fender Tools.
---

# Sauce Town Buildium

Use the hosted Buildium MCP connection for property-management reads and supported, individually approved changes.
The server binds the company and workspace to the approved connection;
never ask a user for a provider URL, provider credential, or a personal token in
chat.

## Setup

Use the plugin's hosted MCP connection. When Codex requests authorization,
open its browser sign-in flow, sign in with the email and regular password
provided by Fender, select the permitted company workspace, and approve access.
Never request a password, provider key, personal token, or OAuth credential in
chat. No helper program or operating-system installer is required.

Call `connection_status` before doing work. Use `buildium_overview`,
`buildium_list_operations`, and `buildium_get_operation` to discover permitted
reads, then `buildium_read` or documented `buildium_find_records` aliases.
Read tools remain GET-only. Keep pagination bounded and filters narrow.

## Recovery

- Authorization expired or revoked: reconnect using Codex's browser sign-in flow.
- Forgotten password: use Forgot password at https://portal.fenderindustries.com/login.
- Permission denied: ask Fender to adjust workspace access; reconnecting cannot grant permissions.
- Provider credential failure: ask Fender to repair the company connection.
- Provider outage or timeout: retry later without rotating credentials.

## Discovery and response handling

- Operation lists contain compact summaries. Follow `nextOffset` to page through
  results and call `buildium_get_operation` for the schema of a selected operation.
  Do not dump every operation schema into the conversation.
- `maintenance`, `workorders`, and `work_orders` resolve to work orders;
  `rentals`, `rental_properties`, and `properties` resolve to rental properties;
  `units` and `rental_units` resolve to rental units. Search supports maintenance.
- Preserve provider field names such as `Id`, `IsActive`, and `Address`. Read
  records inside the returned `data` envelope and inspect provider `statusCode`
  and `pagination`; do not assume a flat response or camelCase fields.
- A successful response with no records is a valid empty result, not a failure.
  State the applied filters and page bounds; do not infer that the whole account
  is empty from a filtered or partial page.
- Tenant/vendor records can include sensitive identity, contact, tax and insurance
  fields. Retrieve only what the task needs and omit unrelated sensitive fields
  from summaries, exports and logs. Never copy full records into debug logs.
- When writing JavaScript tool calls, use `arguments: args` rather than declaring
  a local variable named `arguments`.

## Preparing changes

1. Discover supported changes with `list_write_operations`, using search and pagination. Get the exact schema with `get_write_operation`; never invent an endpoint or input field.
2. Read the current target records and collect missing details. Explain the intended change, affected records or recipients, and financial or destructive effects.
3. Call `prepare_write` with the chosen operation ID and `{ parameters, body }` arguments. This creates a proposal; it does not change provider data.
4. Give the user the returned `approvalUrl`. Check `canApprove` and `approvalRequirement`; an administrator may first need to grant that operation under the client's write permissions at https://portal.fenderindustries.com/admin.
5. The user reviews the exact company, account and fields in the portal and confirms with their password. Never ask for that password in chat, approve through browser automation, or submit the approval endpoint on the user's behalf.
6. Use `write_action_status` after approval. Report success only for `succeeded`, and perform a narrow read-back where supported. A draft is not a completed action.

Proposals expire after 15 minutes. Changing the target or payload requires a new proposal and approval. Do not retry a `running` or `uncertain` action, including by making a new proposal; reconcile with provider records first. Provider responses may include sensitive data: summarize only what is necessary.

Each approval authorizes one exact proposal. Repeated approvals do not grant standing permission. Recurring automation execution is not available in this release. You may help design a workflow, but must not claim it is scheduled or authorized until an explicit, bounded automation policy and execution system exist. Multi-step workflows currently require approval for each mutation.
