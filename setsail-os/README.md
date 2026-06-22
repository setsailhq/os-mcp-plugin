# Setsail OS — Claude plugin

Connects Claude to **Setsail OS** over MCP. This plugin ships the **tools**; the
team's skill **playbooks** live in the PostHog skill store and install separately
as an auto-updating marketplace (see "Skills" below).

## What you get

**Tools** (from the OS MCP server at `https://os.setsail.ca/api/mcp/mcp`):
- `search_records` — find companies, deals, tasks, approvals by keyword (a finder, capped — not for counting)
- `pipeline_summary` — count the pipeline by stage across your book (leads and deals, per-stage counts + value)
- `get_account_context` — strategy, health, billing, timeline, tasks, approvals…
- `search_account_comms` — emails, calls, meeting transcripts
- `log_account_note` — add a note to an account timeline (asks to confirm)
- `list_worksheets` / `read_worksheet` / `create_worksheet` / `revise_worksheet` — build, read, and edit client discovery worksheets and get their fillable URLs

**Skills** — the team's playbooks (`account-prep`, `account-triage`, `clickup`,
`deal-desk`, `deep-dive-worksheet`, `delivery-status`, `log-discipline`,
`quote-desk`, `social`, `website`) are **not bundled here**. They live in the
**PostHog skill store** as the single source of truth and install as a separate,
auto-updating marketplace (`posthog-skill-store`). Edit them in PostHog and Claude
Code pulls updates automatically — no plugin republish. To connect, run PostHog's
`skill-store-install-command` and paste the `/plugin marketplace add …` +
`/plugin install …` lines it returns (each teammate mints their own read-only token).

Everything is scoped to your role and your book, and every call is audit-logged
in the OS (`/settings/audit-log`).

## Install (Claude Code)

```bash
claude plugin marketplace add https://github.com/setsailhq/os-mcp-plugin
claude plugin install setsail-os@setsail-os --scope project
/reload-plugins
```

First use: run `/mcp`, pick **os-api**, and complete the browser sign-in. You log
into the OS (Google/credentials) and approve the connection on a consent screen —
no token to copy. Tokens are stored in your system keychain and refreshed
automatically.

## Claude Desktop / claude.ai (no plugin system)

Plugin packaging (and the PostHog skill-store marketplace) are **Claude Code /
Codex only**. On Desktop / web you connect the MCP server directly and won't get
the skills:

- **claude.ai (web):** Settings → Connectors → Add custom connector → URL
  `https://os.setsail.ca/api/mcp/mcp` (leave OAuth fields blank — the server
  self-registers). You'll be sent to the OS login + consent.
- **Claude Desktop:** use a personal access token from `/settings/mcp` with the
  `mcp-remote` bridge in `claude_desktop_config.json` (see the OS MCP docs).

## Requirements
- A Setsail OS account with role **account_manager or higher**.
- Node.js (for the Desktop `mcp-remote` path only).
