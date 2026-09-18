# SauceTown plugins for Codex

Read-only Buildium and Stannp tools for authorized SauceTown Properties staff,
provided by Fender Industries. This public repository contains only plugins
and skills. Installing them does not grant access to company data.

## Before you start

You need Codex with plugin support and your individual Fender Tools login,
provided by Fender. Sign in at https://portal.fenderindustries.com/login using
your email and regular password. There is no public account registration.
Forgot your password? Use https://portal.fenderindustries.com/reset.

## Install

In a terminal where the Codex CLI is installed, run:

```sh
codex plugin marketplace add https://github.com/calebfender/fender-saucetown-plugins
codex plugin add saucetown-buildium@fender-saucetown
codex plugin add saucetown-stannp@fender-saucetown
```

These commands install skills and hosted MCP connection settings. No separate
Fender desktop application, native installer, provider API key, or manually
created personal token is needed.

Start a new Codex conversation after installation. Select the appropriate
plugin and ask it to check its connection. When authorization is requested,
follow the browser sign-in link, sign in with your Fender Tools account,
select your permitted SauceTown workspace, and approve the connection.
Authorize Buildium and Stannp separately when prompted.

## First checks

For Buildium, ask:

> Use the SauceTown Buildium plugin to check my connection and list up to five properties. Do not change anything.

For Stannp, ask:

> Use the SauceTown Stannp plugin to check my connection and show the current account balance. Do not send mail or change anything.

Buildium tools allow permitted reads only. Stannp supports status, balance and
bounded reporting; sending mail, changing recipients and topping up are not
available. Permissions are checked on the server for each request.

## Manage access

View and disconnect authorized apps at
https://portal.fenderindustries.com/oauth/connections.
Signing out of the website does not itself disconnect an authorized plugin.
If access is denied or no workspace is available, contact Fender to check your
account permissions. Never paste passwords or provider credentials into Codex
chat, an issue, or this repository.

## Connection URLs

The plugins already include these; manual entry is normally unnecessary.

- Buildium: https://tools-mcp.fenderindustries.com/mcp/buildium
- Stannp: https://tools-mcp.fenderindustries.com/mcp/stannp
- Sign-in portal: https://portal.fenderindustries.com/login

## Updates and support

Use Codex's plugin marketplace update flow when Fender publishes an update.
Start a new conversation after updating. Report setup errors to your Fender
contact without including credentials or private company records.

## Verification status

Both providers have passed authenticated live reads through the hosted
platform, and permission/security tests pass. A complete real-client Codex
browser authorization and tool-call acceptance test is still pending.
