---
name: deal-desk
description: Work Setsail deals — list and inspect an account's deals and quotes, move a deal's stage, create or edit a deal, and provision a won deal into client work.
disable-model-invocation: false
---

# Deal desk

Use for sales/pipeline questions about a client — "where's this deal at", "what
deals are open on X", "what's the quote total", "is this ready to provision".

## Steps
1. Resolve the account (via `search_records` if needed).
2. `query_deal` with just the company to LIST the account's deals (id, stage,
   value, quotes, deliverables). Pass a `dealId` to drill into one deal's quotes
   and provisioned work.
3. Before provisioning, optionally run `get_quote_divergences` to check whether
   the current quote still matches the approved plan, and surface any differences.

## Writes (confirm before each — the client will also prompt)
- **`move_deal_stage`** — advance a deal through its stages to won or lost (a lost
  deal takes a reason). Reversible. Marking a deal won does not create work on its
  own — that's `provision_deal`.
- **`update_deal`** — edit a deal (pass dealId) or create one (omit dealId; title
  required, plus optional value, expected close, notes, stage).
- **`provision_deal`** — turn a won deal into live client work. It's a real,
  client-facing action: call once for a **preview**, then re-call with
  `confirm: true` to execute. Surface any blockers it returns.

## Pipeline + scoping reads
- **`pipeline_summary`** — book-wide open deals by stage with total + probability-
  weighted value ("what's my pipeline", "how much is in negotiation", "forecast").
- **`check_workflow`** — before scoping new work, check whether work for that
  service line is already in flight for the account (avoids duplicate scoping).

## Quoting
Quote authoring — propose a range → create → modify → lock — is its own flow;
see the **quote-desk** skill. After a quote is locked and the deal is won,
`provision_deal` turns it into client work.

## Report
For a deal: stage, value, quote status, work provisioned. Keep it scannable.
Everything is scoped to your book and role, and each write is audit-logged; the
client also prompts before each write.
