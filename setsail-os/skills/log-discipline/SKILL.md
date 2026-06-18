---
name: log-discipline
description: When the user wants to record a note, call outcome, or decision to a Setsail account timeline, format it cleanly and confirm the account before writing via log_account_note.
disable-model-invocation: false
---

# Log discipline

Use when the user wants to log/record something to a client's timeline — a call
recap, a decision, a next step, a note.

## Steps
1. **Resolve the account** with `search_records` if you don't have the exact id.
   If it's ambiguous, confirm which client before writing.
2. **Draft a clean note** before logging:
   - One-line summary first, then detail.
   - Capture decisions, owners, and next steps explicitly.
   - Keep it factual — this lands on the account timeline other team members read.
3. **Show the user the drafted note + the target account and ask them to confirm**
   before calling `log_account_note`. The MCP client will also surface its own
   approval prompt for the write — that's expected; don't bypass it.
4. After logging, confirm what was written and to which account.

## Notes
- `log_account_note` writes an internal note attributed to the signed-in user.
  It's reversible, but treat it as a real record — don't log speculation as fact.
- Never log to an account you couldn't resolve. If unsure, ask.
