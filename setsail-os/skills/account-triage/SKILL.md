---
name: account-triage
description: Triage what needs attention across Setsail accounts — read health, pending approvals, and stuck work for a client (or a few) and recommend the next actions, most urgent first.
disable-model-invocation: false
---

# Account triage

Use when the user asks "what needs my attention", "what's at risk", "what should
I do today", or to review the state of one or several accounts.

## Steps
1. **Scope it.**
   - Broad ask ("my book", "anything at risk", "what should I do today") →
     **`get_health_board`** — the book-wide health board, at-risk-first (lowest
     score). Pass `zone` (red/amber/green) or `limit` to narrow. This is your
     portfolio entry point.
   - Specific accounts named → resolve each via `search_records`.
2. For an account that needs a closer look, call `get_account_context` with
   topics `health, approvals, tasks` (add `strategy` if health looks off and you
   need the "why"), and `get_pending_requests` / `get_stuck_cards` for the
   actionable detail.
3. **Rank** findings across accounts by urgency:
   - 🔴 health declining / SLA or renewal risk / client waiting on us
   - 🟡 pending approvals, stuck tasks, stale items
   - 🟢 healthy, no action
4. **Report** as a prioritized list: `Account — issue — recommended action`.
   Keep it scannable; lead with the 🔴 items.

## Notes
- `get_health_board` is book-scoped by role (all-access roles see everything);
  it returns scored, active accounts. Use it for the portfolio view rather than
  guessing which accounts exist.
- If health/approvals came back empty for an account, say it's quiet rather than
  implying you couldn't check.
