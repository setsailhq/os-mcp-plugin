---
name: account-triage
description: Triage what needs attention across Setsail accounts — read health, pending approvals, and stuck work for a client (or a few) and recommend the next actions, most urgent first.
disable-model-invocation: false
---

# Account triage

Use when the user asks "what needs my attention", "what's at risk", "what should
I do today", or to review the state of one or several accounts.

## Steps
1. **Scope it.** If the user named accounts, resolve each via `search_records`.
   If they asked broadly ("my book", "anything at risk"), ask which accounts to
   check, or work through the ones they name — there is no bulk-list tool, so
   triage the specific accounts in play.
2. For each account, call `get_account_context` with topics
   `health, approvals, tasks` (add `strategy` if health looks off and you need
   the "why").
3. **Rank** findings across accounts by urgency:
   - 🔴 health declining / SLA or renewal risk / client waiting on us
   - 🟡 pending approvals, stuck tasks, stale items
   - 🟢 healthy, no action
4. **Report** as a prioritized list: `Account — issue — recommended action`.
   Keep it scannable; lead with the 🔴 items.

## Notes
- Don't fabricate a portfolio view — only report on accounts you actually pulled.
- If health/approvals came back empty for an account, say it's quiet rather than
  implying you couldn't check.
