# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

This is a distribution-only repo for the Cashew MCP server. It has no server code, build, or tests. The server is hosted remotely at `https://www.cashew.sg/mcp` (streamable HTTP, no API key). This repo holds only the manifests that let clients discover and install that remote server, plus the public README.

The server's source lives in the `cashew` monorepo (a sibling checkout, `../cashew`), under `services/rate-engine/src/mcp/`. That code is the source of truth for everything this repo describes:
- `server.ts`: `serverInfo()` holds the server name, version, title and icons. `PROTECTED_TOOL_NAMES` lists the tools that need sign-in. The `registerTool` calls, plus the shared calculator catalog in `packages/lib-types/src/rate-engine/calculator.catalog.ts`, make up the tool list.
- `server-card.ts`: the SEP-2127 server card served at `https://www.cashew.sg/mcp/server-card`, which the site's `/.well-known/ai-catalog.json` points to. The card reads its name and version from `serverInfo()`.

## How the manifests fit together

Each file serves a different distribution channel, and all of them point at the same remote URL:

- `.mcp.json`: the MCP server definition that Claude Code loads when the plugin is installed (`"type": "http"`).
- `.claude-plugin/plugin.json`: the Claude Code plugin metadata. The plugin is named `cashew-mcp`.
- `.claude-plugin/marketplace.json`: makes this repo a Claude Code marketplace named `cashew`, with a single plugin whose `source` is the repo root (`./`). That is why users install it with `claude plugin marketplace add cashew-sg/cashew-mcp` and then `claude plugin install cashew-mcp@cashew`.
- `.cursor-plugin/plugin.json` and `mcp.json`: the same plugin for the Cursor Marketplace. Cursor reads `mcp.json` (no dot) where Claude Code reads `.mcp.json`, so the server URL lives in both.
- `assets/icon.svg`: the square icon. The Claude manifest points at it as `icon` (read by Anthropic's plugin directory, not by Claude Code itself), and the Cursor manifest points at it as `logo`. A copy is served at `https://www.cashew.sg/icon-cashew-mcp.svg` (from `cashew/apps/alice/public/`) for portals that want a logo URL.
- `server.json`: the entry for the official MCP Registry (schema `2025-12-11`, name `sg.cashew/mcp`, remote type `streamable-http`). It is published and live in the Registry.
- `README.md`: the public docs. Its tool list is the human-facing catalogue.

When you change any of these, keep them consistent with each other:
- The server URL, repo URL (`github.com/cashew-sg/cashew-mcp`), homepage (`/intro-to-mcp`), and privacy URL.
- The plugin name `cashew-mcp` and marketplace name `cashew`. These appear in the install commands in the README.
- The tool list in `README.md` and the server's registered tools (23 at the time of writing). The README has fallen behind before. If the server is connected in your session, its `mcp__cashew__*` tools show the current list.
- `server.json`'s `name` and `version` must match `serverInfo()` in `server.ts`, because the server card reports that name and version. Both `plugin.json` files (Claude and Cursor) follow the same version number. The Registry rejects a second publish of an existing version, so bump all four together before you republish.
- `server.json`'s `description` and `icons` mirror the server card. The Registry caps the description at 100 characters.

## Validating changes

There is nothing to build. To check the plugin and marketplace manifests, run:

```
claude plugin validate .
```

This file lives at `.claude/CLAUDE.md` rather than the repo root. A root `CLAUDE.md` makes the validator and the plugin directory warn that it is not loaded as plugin context. Claude Code still loads it from `.claude/` for people working on the repo.

`claude plugin validate .` should then pass with nothing to report. Keep `privacyPolicyUrl` in `.claude-plugin/plugin.json`: without it the validator raises `PRIVACY_URL_MISSING`. The plugin directory's own checks still show two warnings, and both fields stay. It flags `icon` as a field from another tool's manifest, but it reads `icon` for the listing. It may also flag `privacyPolicyUrl` as an unrecognised field, even though the local validator asks for it.

To check the live server end to end, install the plugin locally (or rely on `.mcp.json`) and call one of the no-auth tools, such as `list_banks`.

## Domain conventions (for README and tool copy)

- All amounts are in SGD.
- Interest rates passed in to calculators are whole-number percentages (`3.5` means 3.5% p.a.). Rates returned by `calculate_package_schedule` and `get_index_quotes` are decimal fractions (`0.0356`).
- All tools are read-only, and the server is stateless.
- `recommend_packages` and `get_property_valuation` require the user to sign in with a Cashew account. All other tools work without authentication.
- README voice: British spelling ("amortisation"), short benefit-led sentences, no em dashes in prose.
