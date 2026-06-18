---
name: deal-desk
description: Work Setsail deals — list/inspect an account's deals and quote-vs-strategy drift, move a deal's pipeline stage, create or edit a deal, and provision a won deal into client work.
disable-model-invocation: false
---

# Deal desk

Use for sales/pipeline questions about a client — "where's this deal at", "what
deals are open on X", "what's the quote total", "did the quote drift from the
strategy", "is this ready to provision".

## Steps
1. Resolve the account (via `search_records` if needed).
2. `query_deal` with just the company to LIST the account's deals (id, stage,
   value, # quotes, # deliverables). Pass a `dealId` to drill into one deal's
   linked quotes (incl. locked + total) and provisioned deliverables.
3. Before a deal is provisioned, run `get_quote_divergences` to check the live
   quote against the strategy's approved scope — surface any added/removed/
   repriced line items.

## Writes (confirm before each — the client will also prompt)
- **`move_deal_stage`** — advance a deal (deep_dive → … → closed_won / closed_lost).
  Find the dealId via `query_deal` first. closed_lost takes a lostReason.
  Reversible. Moving to closed_won does NOT create work — that's `provision_deal`.
- **`update_deal`** — edit a deal (pass dealId) or create one (omit dealId; title
  required: value, expectedClose, notes, stage).
- **`provision_deal`** — turn a won deal's locked quote into live Deliverables and
  flip the client portal live. Run AFTER closed_won + lock. It's a real,
  client-facing action: call once for a **preview**, then re-call with
  `confirm: true` to execute. Surface any blockers it returns.

## Report
For a deal: stage, value, quote status (locked? total), deliverables. For
divergences: list them plainly and note that Accept/Dismiss is a deliberate
strategist action in the engagement-scope panel, not something you do here.

## Note
**Quote writes** (create/modify/lock a quote) aren't on this connector yet — do
those in the OS for now. Everything is scoped to your book and role, and each
write is audit-logged.
