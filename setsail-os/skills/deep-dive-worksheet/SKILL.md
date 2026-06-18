---
name: deep-dive-worksheet
description: Create, find, and edit a client Deep Dive Worksheet in Setsail OS — the pre-filled discovery doc (confirmed facts + evidence-anchored questions) sent to a client to complete before strategy work.
disable-model-invocation: false
---

# Deep Dive Worksheet

Use when the user asks to build / create / generate a deep dive worksheet (or
discovery brief / client intake), to get a worksheet's link, or to change the
questions on one ("remove the X question", "add a question about Y", "reword Z").

A worksheet is a knowledge-transfer doc: confirmed facts (what the client already
told us, pre-filled) plus a tight set of evidence-anchored open questions only the
client can answer. The goal is for the client to feel "you already did 70% of
this" and finish the rest in 15–20 minutes.

## Tools
- `list_worksheets` — latest pre/post-call worksheets for a client + their public URLs and counts.
- `create_worksheet` — generate + persist one from the client's calls + research; returns the fillable URL.
- `revise_worksheet` — edit the latest worksheet's QUESTIONS from plain-language feedback (confirmed facts are preserved; URL stays the same).

## Steps
1. **Resolve the account.** If you don't have the exact client, call
   `search_records` first. If several match, ask — never guess.
2. **Check what exists.** Call `list_worksheets` to see if there's already a
   worksheet (and grab its URL) before creating a duplicate.
3. **Create or revise:**
   - To make one: `create_worksheet` with the client. Use `post_call` (default,
     the client-facing fillable form) unless they're prepping to RUN the call —
     then `pre_call`.
   - To change one: `revise_worksheet` with the change described in plain
     language. Examples: *"remove the 12-month success question"*, *"add a
     seasonality question to Budget"*, *"reword the Airbnb question to focus on
     the guest side"*.
4. **Hand back the public URL** so the user can send it to the client.

## Notes
- `revise_worksheet` steers **questions only** — it never alters confirmed facts,
  and it keeps the **same public URL** (safe to share before editing).
- Generation reads the account's transcripts + research; if a client has no calls
  on file, say so — the worksheet will lean on research and need confirmation.
- Worksheet edits are reversible drafts; sending the link to the client is a
  human action. Don't claim it was sent.
- Scoped to the user's book and audit-logged like every OS tool.
