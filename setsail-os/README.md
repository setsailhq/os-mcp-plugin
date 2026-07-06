# Setsail OS — Claude plugin

Connects Claude to **Setsail OS** over MCP. One install gets you both the
**tools** (this plugin's MCP server) and the **`phs` bridge skill**, which
routes to the team's playbooks in the live PostHog skill store (see "Skills"
below) — no second install step.

## What you get

**Tools** (from the OS MCP server at `https://os.setsail.ca/api/mcp/mcp`):
- `search_records` — find companies, deals, tasks, approvals by keyword (a finder, capped — not for counting)
- `pipeline_summary` — count the pipeline by stage across your book (leads and deals, per-stage counts + value)
- `get_account_context` — strategy, health, billing, timeline, tasks, approvals…
- `search_account_comms` — emails, calls, meeting transcripts
- `get_sales_summary` — org-wide revenue metrics (MRR/ARR/NRR/cash), top clients, and per-rep commissions (sales_manager+)
- `log_account_note` — add a note to an account timeline (asks to confirm)
- `list_worksheets` / `read_worksheet` / `create_worksheet` / `revise_worksheet` — build, read, and edit client discovery worksheets and get their fillable URLs

**Skills** — the team's playbooks (`account-prep`, `account-triage`, `clickup`,
`deal-desk`, `deep-dive-worksheet`, `delivery-status`, `log-discipline`,
`quote-desk`, `social`, `website`, plus `compass-pm`) are content-hosted in the
**PostHog skill store** (project "Backend OS"), not bundled as static files in
this plugin. What IS bundled here is `skills/phs/SKILL.md` — a thin bridge that
this plugin ships automatically, so installing `setsail-os` gets you the
bridge with no separate skill-store install. The bridge routes `/phs
<skill-name>` (or a matching natural-language request) to a live `skill-get`
call against PostHog. Edit a skill in PostHog and every teammate gets the
update on their next `skill-get` — no plugin republish, no `/reload-plugins`.
The bridge itself only changes when we `publish:plugin` again (rare); the
skill content it fetches changes live (common).

First use of any playbook also requires the **PostHog MCP server** connected
in Claude Code (`/mcp` → add PostHog, or `claude mcp add posthog ...`) — the
bridge calls PostHog's `skill-*` tools directly.

Restored 2026-07-06: these 10 skills existed only in git history
(`mcp-plugin/setsail-os/skills/*` in the main OS repo, deleted at commit
`ba2dbde2` when the PostHog migration was decided) — the PostHog store itself
had been left empty since. All 10 were re-ported from `ba2dbde2^` plus
`compass-pm`, giving 11 skills live in PostHog today. The bridge skill was
folded into this plugin the same day so one install covers both tools and
skill access.

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

To use the playbook skills (not just the raw tools), also connect the
**PostHog MCP server** via `/mcp` — the bundled `phs` bridge needs it to reach
the skill store.

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
