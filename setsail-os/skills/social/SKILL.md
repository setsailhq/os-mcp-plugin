---
name: social
description: Run Setsail social media from Claude — calendar, analytics, concepts, scheduling, inbox replies, approvals, publishing, delivery cycles, and playbooks for a client.
disable-model-invocation: false
---

# Social

Use for any social-media question or action on a client — "what's scheduled",
"how are we performing", "draft this week's posts", "reply to this DM", "approve
these", "publish it", "kick off this month's cycle". Resolve the account first
(via `search_records` if you don't have the exact name); every tool takes a
`company` arg except the two agency-wide reports.

## Read / report first
- **`get_social_analytics`** — current follower/engagement snapshot.
- **`get_social_report`** — top posts + totals over a date window (optional platform).
- **`list_social_calendar`** — scheduled/published posts for a month.
- **`get_social_playbook`** — the strategy's social playbook (pillars, cadence).
- **`get_connection_health`** — are the client's platform connections live/expired.
- **`list_social_assets`** — media library (filter by folder/kind/status).
- Agency-wide (no `company`): **`get_social_team_performance`**, **`get_message_breakdown`** (date range).

## Plan & schedule
- **`create_social_concept`** → ideas as drafts; **`list_social_concepts`** → review; **`promote_social_concept`** → move concepts onto the calendar.
- **`schedule_social_post`** (one) / **`create_content_plan`** (batch) → put posts on the calendar.
- **`update_social_post`** / **`reschedule_social_post`** → edit / move a post.
- **`attach_asset_to_post`** → attach a library asset.

## Engage (inbox)
- **`list_inbox_messages`** → triage queue; **`reply_to_message`** (records the reply + flips the thread — does NOT post to the live platform); **`triage_messages`** → bulk assign/close/tag/note.

## Approvals & delivery cycle
- **`list_pending_approvals`** → what's awaiting sign-off; **`submit_post_for_approval`**, **`decide_post_approval`** (approve/reject/request_revision — reject needs a comment).
- **`list_social_delivery`** → deliverable + recent cycles; **`generate_social_cycle`** (build a month), **`approve_cycle_plan`** (⚠️ mints live ClickUp work — needs `confirm:true`), **`set_cycle_status`**.
- **`run_playbook_step`** / **`approve_social_playbook`** → build + activate the playbook.
- **`release_posts_to_client`** → lead-approve so posts surface in the client portal.

## Publish
- **`publish_social_post`** / **`retry_publish`** — ⚠️ posts to LIVE platforms; needs `confirm:true`. Publishing is env-gated — if it's off, the result records a `flag_off` failure instead of sending; surface that. **`get_publish_failures`** → recent failures.

## Connections
- **`get_connection_health`**, **`start_social_connection`** → returns a link to send the client to connect their accounts.

## Notes
Most writes are account_manager-gated, scoped to your book, and audit-logged.
Anything that hits a live platform (`publish`/`retry`) or mints live work
(`approve_cycle_plan`) requires `confirm:true` and the client will also prompt.
Ground every metric in tool output; if a read is empty, say so.
