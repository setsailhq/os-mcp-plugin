---
name: delivery-status
description: Report an account's delivery health from Setsail OS — plan progress, stuck cards, feature usage vs plan, and pending client requests — and call out what needs action.
disable-model-invocation: false
---

# Delivery status

Use when the user asks how delivery is tracking for a client — "how are we doing
on X", "what's shipping", "are we behind", "what's stuck", "usage vs plan".

## Steps
1. Resolve the account (via `search_records` if you don't have the exact name).
2. `get_plan_progress` — the cycle-by-cycle overview and overall delivery chip.
3. If anything looks yellow/red, `get_stuck_cards` for the blockers + suggested
   actions, and `get_pending_requests` for unactioned client asks.
4. For "did we deliver what they paid for" questions, `get_feature_usage`
   (committed vs shipped this period).
5. Need one specific dimension (just tasks, just approvals)? `query_account`
   with that `dataType`.

## Report
Lead with the overall chip, then: **shipping now**, **stuck / at risk** (with the
suggested action), **usage vs plan** if relevant. Keep it scannable. Ground every
number in tool output — if a read came back empty, say so rather than guessing.
