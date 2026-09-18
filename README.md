# SauceTown plugins for Codex

Buildium and Stannp reads and human-approved changes for authorized SauceTown Properties staff,
provided by Fender Industries. This public repository contains only plugins
and skills. Installing them does not grant access to company data.

## Before you start

You need Codex with plugin support and your individual Fender Tools login,
provided by Fender. Sign in at https://portal.fenderindustries.com/login using
your email and regular password. There is no public account registration.
Forgot your password? Use https://portal.fenderindustries.com/reset.

## Install

For individual Codex desktop accounts, add this public marketplace once on each computer. In **Add plugin marketplace**, enter:

- Source: `calebfender/fender-saucetown-plugins`
- Git ref: `main`
- Sparse paths: leave empty

A bare `github.com/...` is not a valid source; use the owner/repository form above. If `fender-saucetown` already exists from another source, remove that marketplace entry before adding this one.

Alternatively, in a terminal where the Codex CLI is installed, run:

```sh
codex plugin marketplace add https://github.com/calebfender/fender-saucetown-plugins
```

Restart Codex, open **Plugins**, and choose the **Fender Saucetown** marketplace.
Open **Saucetown Buildium** and click **Install**. Repeat for **Saucetown Stannp**.
Plugin-directory labels can vary with the app version. This marketplace setup
is documented in [OpenAI's plugin guide](https://developers.openai.com/plugins/build/plugins#add-a-marketplace-from-the-cli).

If you prefer to install through the terminal, the equivalent commands are:

```sh
codex plugin add saucetown-buildium@fender-saucetown
codex plugin add saucetown-stannp@fender-saucetown
```

The plugins contain skills and hosted MCP connection settings. No separate
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

Version 0.2.0 also discovers supported changes and prepares proposals. Fender must grant your write permissions separately. Each proposal gives you an approval link at https://portal.fenderindustries.com/approvals. Review the company, account and exact details, then confirm there with your portal password. The plugin cannot approve on your behalf. Never paste the password into chat.

This release supports individually approved Buildium changes and Stannp letter/postcard proposals. Six Buildium JSON Patch operations and other Stannp mutations are not included. Recurring automatic workflows are not yet available. A draft or uncertain result must never be treated as a completed change or blindly retried.

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

Both providers have passed authenticated live reads through the hosted platform, including real-client Codex use. The individual write flow is covered by automated tests and synthetic browser/container tests. Live provider mutations have not been performed during this release validation. Test a representative client-approved sandbox or provider-test workflow before live production changes.
