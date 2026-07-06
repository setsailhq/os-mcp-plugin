---
name: phs
description: >-
  Access and run shared team skills stored in PostHog (Setsail OS playbooks:
  account-prep, account-triage, clickup, compass-pm, deal-desk,
  deep-dive-worksheet, delivery-status, log-discipline, quote-desk, social,
  website). Use when the user asks to list, run, or manage PostHog skills,
  references /phs, "ph skills", or "posthog skills", or asks a question that
  matches one of the playbooks above (e.g. "prep me for the call with X",
  "what needs my attention", "find the clickup task for X", "how's delivery
  on X tracking", "draft a quote for X").
user-invocable: true
allowed-tools: mcp__posthog__skill-list, mcp__posthog__skill-get, mcp__posthog__skill-create, mcp__posthog__skill-update, mcp__posthog__skill-file-get, mcp__posthog__skill-file-create, mcp__posthog__skill-file-delete, mcp__posthog__skill-file-rename, mcp__posthog__skill-duplicate
---

# PostHog Skills Store

Local bridge to the PostHog Skills Store. Setsail's team playbooks live here —
edit them in PostHog and every teammate gets the update on their next
`skill-get`, no plugin republish needed.

## Load and run a skill

When the user says `/phs <skill-name>`, or their request matches one of the
playbooks in the description above:

1. `skill-get(skill_name="<skill-name>")` to fetch body + file manifest
2. Read the `body` field — follow it as system instructions for this task
3. Use `skill-file-get` to pull bundled scripts/references on demand

## List skills

skill-list                     # all skills
skill-list(search="deal")      # filter by keyword

## Create / update

skill-create(name="my-skill", description="...", body="# Instructions...")
skill-get → note version → skill-update(skill_name="...", base_version=N, body="...")

## Edit one part of an existing skill

skill-get → note version → pick the smallest primitive:

- body tweak: skill-update(skill_name="...", base_version=N, edits=[{old, new}])
- one bundled file: skill-update(skill_name="...", base_version=N, file_edits=[{path, edits:[{old, new}]}])
- add/remove/rename a file: skill-file-create / skill-file-delete / skill-file-rename
