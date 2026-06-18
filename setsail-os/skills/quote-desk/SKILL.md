---
name: quote-desk
description: Scope and author Setsail quotes — propose a services bundle + price range, create or edit a draft quote from catalog presets, and lock the agreed scope.
disable-model-invocation: false
---

# Quote desk

Use for quoting/scoping a client — "what would X cost", "draft a quote for Brand
+ SEO", "lock that quote in", "change the SEO tier". Needs the **sales_rep role
or above** (execs/sales); account managers can read deals but not author quotes.

## Scope first (optional)
- **`propose_services_range`** — turn a free-text ask ("they want SEO and paid
  social") into a recommended bundle + price range from the catalog. Use before
  committing to line items.

## Author the quote
1. Resolve the account (`search_records` if you don't have the exact name).
2. **`create_quote`** — one line item per service using `presetKey` (e.g.
   `brand-system`, `seo-growth`, `ppc-multi-leadgen`, `lifecycle`) + quantity.
   **Pick ONE tier per service.** It returns a **preview** first — re-call with
   `confirm: true` to save the draft. dealId is optional (auto-resolves/creates).
3. **`modify_quote`** — adjust title / line items / total on a draft (refuses if
   the quote is locked or already accepted).
4. **`lock_quote`** — freeze the agreed scope once the client says yes (Compose
   Proposal and `provision_deal` both read the locked quote). `unlock_quote` to revise.

## Then hand off
Use the **deal-desk** skill to `move_deal_stage` → `closed_won`, then
`provision_deal`. Run `get_quote_divergences` first to confirm the quote still
matches the strategy's approved scope.

## Notes
One quote per deal — `create_quote` overwrites the existing draft. Every line is
validated against the catalog. Quote writes are gated to sales roles and
audit-logged, and the client prompts before each.
