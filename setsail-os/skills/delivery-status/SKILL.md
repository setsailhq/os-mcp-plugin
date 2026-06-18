---
name: delivery-status
description: Report an account's delivery status from Setsail OS — progress, stuck work, usage vs plan, and pending client requests — and call out what needs action.
disable-model-invocation: false
---

# Delivery status

Use when the user asks how delivery is tracking for a client — "how are we doing
on X", "what's shipping", "are we behind", "what's stuck", "usage vs plan".

## Steps
1. Resolve the account (via `search_records` if you don't have the exact name).
2. `get_plan_progress` — the overall delivery progress overview.
3. If anything looks at risk, `get_stuck_cards` for the blockers + suggested
   actions, and `get_pending_requests` for unactioned client asks.
4. For "did we deliver what they paid for" questions, `get_feature_usage`
   (planned vs delivered this period).
5. Need one specific dimension (just tasks, just approvals)? `query_account`
   with that `dataType`.
6. Before scoping new work, `check_workflow` to see whether work for that
   service line is already in flight (avoids duplicate scoping).

## Report
Lead with the overall status, then: **shipping now**, **stuck / at risk** (with
the suggested action), **usage vs plan** if relevant. Keep it scannable. Ground
every number in tool output — if a read came back empty, say so rather than
guessing.
