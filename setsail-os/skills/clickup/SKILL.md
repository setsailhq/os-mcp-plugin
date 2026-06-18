---
name: clickup
description: Find and update Setsail ClickUp tasks from Claude — search the synced task mirror, comment, update fields/status/assignees, and edit plan items (which sync back to ClickUp).
disable-model-invocation: false
---

# ClickUp & plan items

Use to find delivery tasks, leave a comment, change a task's status/assignees,
or adjust the plan for a client — "find the task for X", "comment on this task",
"move it to in-progress", "reassign to …", "update the brief for these pages".

## Find first
- **`find_clickup_task`** — search the synced ClickUp mirror by keyword; pass an
  optional `company` to scope to that account's book. Returns task ids you pass
  to the write tools.

## Task writes (live ClickUp — the client will prompt before each)
- **`add_clickup_comment`** — append a comment to a task (`clickupTaskId`, `comment`, `notify?`).
- **`update_clickup_task`** — change `status`, `dueDate`, assignees, or the description. ⚠️ `description` **replaces** the body wholesale — use `appendToDescription` to add without overwriting. Assignees are given by name/email and resolved to ClickUp members.

## Plan items (account-scoped; edits push to the paired ClickUp tasks)
- **`update_plan_items`** — match plan items by `criteria` (title contains, page slug, deliverable, keyword, or explicit ids) and apply `changes` (title, keywords, CTA tier, scope fields) with a `revisionNote`. Pushes the new title/brief to the paired ClickUp task.
- **`undo_last_plan_change`** — revert the last plan revision batch for the account (also re-pushes to ClickUp).

## Notes
Task tools take a raw `clickupTaskId` (not account-scoped) — get it from
`find_clickup_task`. Plan-item tools take a `company`. All are account_manager-
gated and audited; every write hits live ClickUp, so be precise and prefer
`appendToDescription` over `description` unless a full rewrite is intended.
