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

## Response handling

Provider responses retain their original data types and may be nested under
`data` and `statusCode`. Balance values may be decimal strings such as `"23.0200"`.
Validate and convert them to a finite decimal value before numeric comparisons;
use decimal-safe arithmetic for money. Do not treat a missing or invalid balance
as zero, infer a currency that was not returned, or compare money as text.
A successful empty reporting result means no matching records for the requested
period. It is not a provider failure. Keep reporting bounded and omit unrelated
personal data from summaries and logs.
