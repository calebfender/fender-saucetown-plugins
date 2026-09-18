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
