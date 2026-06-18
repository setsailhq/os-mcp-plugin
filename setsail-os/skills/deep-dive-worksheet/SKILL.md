---
name: deep-dive-worksheet
description: Create, find, and edit a client discovery worksheet in Setsail OS — pre-filled from prior calls and research, with open questions for the client to complete before strategy work.
disable-model-invocation: false
---

# Discovery worksheet

Use when the user asks to build / create / generate a discovery worksheet (or
deep dive / client intake), to get a worksheet's link, or to change the questions
on one ("remove the X question", "add a question about Y", "reword Z").

A worksheet pre-fills what's already known about the client and lists the
remaining open questions for them to answer.

## Tools
- `list_worksheets` — latest worksheets for a client + their public URLs and counts.
- `create_worksheet` — generate + persist one from the client's calls + research; returns the fillable URL.
- `revise_worksheet` — edit the latest worksheet's questions from plain-language feedback (pre-filled facts are preserved; URL stays the same).

## Steps
1. **Resolve the account.** If you don't have the exact client, call
   `search_records` first. If several match, ask — never guess.
2. **Check what exists.** Call `list_worksheets` before creating a duplicate.
3. **Create or revise:**
   - To make one: `create_worksheet` with the client. Choose the client-facing
     version by default, or the internal version when prepping to run the call.
   - To change one: `revise_worksheet` with the change described in plain
     language (add / remove / reword a question).
4. **Hand back the public URL** so the user can send it to the client.

## Notes
- `revise_worksheet` changes the questions only — it leaves the pre-filled facts
  alone and keeps the **same public URL** (safe to share before editing).
- Generation reads the account's calls + research; if a client has no calls on
  file, say so — the worksheet will lean on research and need confirmation.
- Worksheet edits are reversible; sending the link to the client is a human
  action — don't claim it was sent.
- Scoped to the user's book and audit-logged like every OS tool.
