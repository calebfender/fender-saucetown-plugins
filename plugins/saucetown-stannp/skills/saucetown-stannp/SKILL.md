---
name: saucetown-stannp
description: Use the read-only Sauce Town Stannp connection through Fender Tools.
---

# Sauce Town Stannp

Use the hosted Stannp MCP connection only for connection status, account
balance, and bounded reporting. Mailing, recipient mutation, top-ups, SMS,
uploads, and provider-test operations are intentionally unavailable.

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
