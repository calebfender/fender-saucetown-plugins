---
name: saucetown-buildium
description: Use the read-only Sauce Town Buildium connection through Fender Tools.
---

# Sauce Town Buildium

Use the hosted Buildium MCP connection for read-only property-management work.
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
All operations are GET-only. Keep pagination bounded and filters narrow.

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
