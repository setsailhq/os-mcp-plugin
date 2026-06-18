---
name: deal-desk
description: Work Setsail leads and deals — count the pipeline by stage ("how many leads in New", "what's in negotiation"), list/inspect an account's deals and quotes, move a LEAD or DEAL between its stages, convert a won lead to a deal, create/edit a deal, and provision a won deal into client work.
disable-model-invocation: false
---

# Deal desk

Use for sales/pipeline questions — both book-wide ("how many leads in the New
category", "what's my pipeline", "how much is in negotiation") and per-client
("where's this deal at", "what deals are open on X", "is this ready to
provision"). Two pipelines exist: **lead** and **deal**, each with its own
stages — see "Pipeline + scoping reads" and "Writes" below.

## Steps
0. **Book-wide count / "how many leads/deals in <stage>" / "my pipeline"?** Go
   straight to `pipeline_summary` — it's book-scoped and needs NO account. Do
   NOT use `search_records` to count (it's a capped keyword finder, max 20).
1. For a specific client, resolve the account (via `search_records` if needed).
2. `query_deal` with just the company to LIST the account's deals (id, stage,
   value, quotes, deliverables). Pass a `dealId` to drill into one deal's quotes
   and provisioned work.
3. Before provisioning, optionally run `get_quote_divergences` to check whether
   the current quote still matches the approved plan, and surface any differences.

## Writes (confirm before each — the client will also prompt)
- **`move_deal_stage`** — advance a DEAL through its stages (deep_dive →
  strategy_presentation → proposal_scoped → negotiating → closed_won/closed_lost/
  long_term_followup) to won or lost (a lost deal takes a reason). Reversible.
  Marking a deal won does not create work on its own — that's `provision_deal`.
- **`move_lead_stage`** — move a LEAD through its OWN stages (new → qualifying →
  qualified → discovery_call → closed_won/closed_lost/disqualified/nurture). Leads
  are positioned by `leadStage`, a different field than a deal's `stage` — so use
  THIS for leads and `move_deal_stage` for deals. Moving a lead to closed_won also
  auto-creates a deal in deep_dive. Reversible.
- **`convert_lead_to_deal`** — promote a won lead into the deal pipeline (new deal
  in deep_dive, copies value/owner/contacts). The lead must be at closed_won.
- **`update_deal`** — edit a deal (pass dealId) or create one (omit dealId; title
  required, plus optional value, expected close, notes, stage).
- **`provision_deal`** — turn a won deal into live client work. It's a real,
  client-facing action: call once for a **preview**, then re-call with
  `confirm: true` to execute. Surface any blockers it returns.

## Pipeline + scoping reads
- **`pipeline_summary`** — book-wide pipeline grouped by **pipeline AND stage**.
  There are TWO pipelines, each with its own stages:
  - **lead**: `new` → `qualifying` → `qualified` → `discovery_call` → `closed_won`
    (+ terminal `closed_lost` / `disqualified` / `nurture`)
  - **deal**: `deep_dive` → `strategy_presentation` → `proposal_scoped` →
    `negotiating` → `closed_won` (+ terminal `closed_lost` / `long_term_followup`)

  Returns per-stage counts, total value, and probability-weighted open value.
  This is the right tool for **counting** — "how many leads in the New category",
  "what's in negotiation", "how many closed won", "what's my pipeline / forecast".
  Pass `pipeline` ("lead" | "deal") to restrict to one; `includeClosed: false` to
  drop terminal stages. Do NOT use `search_records` to count by stage — it's a
  capped keyword finder (returns at most 20 per type and annotates "showing N of
  TOTAL"), not a counter.
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
