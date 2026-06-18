---
name: website
description: Work a Setsail website/SEO deliverable from Claude — read sitemap/pages/copy, audit pages, edit copy & meta in the OS, propose sitemap changes, and queue brief/copy generation.
disable-model-invocation: false
---

# Website & copy

Use for website/SEO delivery on a client — "what pages do we have", "show the
copy for X", "audit this page", "tweak this section", "fix the meta title", "add
these pages", "generate briefs/copy". Resolve the account first; every tool takes
a `company` arg.

## Read
- **`list_copy_pages`** — pages on the deliverable + audit-summary counts.
- **`read_page_copy`** — a page's sections, HTML, meta, brief, and audit (by `pageId` / `slug`).
- **`run_audit_for_page`** — refresh + return the SEO/quality audit for a page.
- **`propose_sitemap_changes`** — a recommended sitemap diff for a directive (read-only; nothing is applied).
- **`read_push_report`** — the latest Webflow push result for a page.

## Edit copy (OS-only — these never touch live Webflow)
- **`edit_page_copy_sections`** — add/replace/remove/reorder sections (ops).
- **`update_page_fields`** — page-level fields.
- **`update_page_meta`** — meta title / description.
- **`replace_text_in_page`** — literal find/replace within section bodies.
- **`revert_page_edit`** — roll back the last edit(s).
All take a `revisionNote`; support `dryRun` for a preview; and change copy in the
OS only. Pushing to the live site is a **separate** step done in the OS (the
Webflow push flow is intentionally not on this connector).

## Sitemap & generation
- **`add_pages`** — propose new pages (reversible ghost rows; strategist-gated).
- **`approve_pages`** — ⚠️ applies sitemap changes (creates/archives pages); strategist-gated, needs `confirm:true` (preview first).
- **`generate_briefs_for_pages`** / **`generate_copy_for_pages`** — queue brief/copy generation jobs (async; returns queued/skipped counts — the work runs on the pipeline).

## Notes
Reads are account_manager-gated; copy edits account_manager; sitemap apply +
page generation are strategist-gated. Everything is book-scoped and audited.
Copy edits are OS-only and reversible (`revert_page_edit`); publishing to the
live website happens in the app, not here.
