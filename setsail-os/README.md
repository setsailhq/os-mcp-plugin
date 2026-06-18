# Setsail OS — Claude plugin

Connects Claude to **Setsail OS** over MCP and ships playbooks so Claude uses the
OS the way the team does.

## What you get

**Tools** (from the OS MCP server at `https://os.setsail.ca/api/mcp/mcp`):
- `search_records` — find companies, deals, tasks, approvals by keyword
- `get_account_context` — strategy, health, billing, timeline, tasks, approvals…
- `search_account_comms` — emails, calls, meeting transcripts
- `log_account_note` — add a note to an account timeline (asks to confirm)
- `list_worksheets` / `create_worksheet` / `revise_worksheet` — build + edit client deep dive worksheets and get their fillable URLs

**Skills** (auto-invoked when relevant):
- `account-prep` — meeting / "what's going on with X" briefs
- `account-triage` — what needs attention, ranked by urgency
- `deep-dive-worksheet` — create / find / edit a client deep dive worksheet
- `log-discipline` — clean, confirmed note logging

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

Skills + plugin packaging are **Claude Code only**. On Desktop / web you connect
the MCP server directly:

- **claude.ai (web):** Settings → Connectors → Add custom connector → URL
  `https://os.setsail.ca/api/mcp/mcp` (leave OAuth fields blank — the server
  self-registers). You'll be sent to the OS login + consent.
- **Claude Desktop:** use a personal access token from `/settings/mcp` with the
  `mcp-remote` bridge in `claude_desktop_config.json` (see the OS MCP docs).

## Requirements
- A Setsail OS account with role **account_manager or higher**.
- Node.js (for the Desktop `mcp-remote` path only).
