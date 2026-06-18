---
name: account-prep
description: Prep for a client meeting or answer "what's going on with <client>" using Setsail OS — pull account context, strategy, health, and recent emails/calls, then synthesize.
disable-model-invocation: false
---

# Account prep

Use when the user asks to prepare for a meeting/call with a client, or asks a
status / "what's going on with X" / "catch me up on X" question about an account.

## Steps
1. **Resolve the account.** If you don't already have the exact account, call
   `search_records` with the client's name to get the id. If multiple match, ask
   the user which one — never guess.
2. **Pull context.** Call `get_account_context` for that client with the topics
   you need — default to `setup, strategy, health, tasks, approvals`; add
   `billing` for commercial conversations, `seo` for delivery reviews.
3. **Pull recent comms.** Call `search_account_comms` for the client — pass the
   meeting topic as the query if there is one, otherwise omit it to list recent
   calls/emails.
4. **Synthesize** into a tight brief:
   - **Where things stand** — strategy + health in 2-3 lines
   - **Open items** — pending approvals, stuck tasks, anything awaiting them or us
   - **Recent signal** — what they last said / what changed (cite the call/email)
   - **Suggested talking points** — 3-5 bullets

## Notes
- Account reads are scoped to the user's book; if a client isn't found, it may
  not be theirs — say so rather than inventing data.
- Always ground claims in what the tools returned. If a topic came back empty,
  say "no data" rather than guessing.
