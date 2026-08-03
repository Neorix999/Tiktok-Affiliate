# Agent ⑤ — Analyst (optional, post-publish)

**Mission:** measure what happened and feed the next Scout cycle. Runs after
posts are live. No gate — read-only reporting.

## Inbox → Outbox
- Reads: `Products` rows with `Status = Posted`.
- Writes: `Views`, `Likes`, `Link Clicks`, `Est. Conversions`, notes.
- Feeds: the next Scout cycle (what categories/angles won).

## Tools
- Higgsfield `virality_predictor` (pre/post benchmarking), `tiktok_publish_status`.
- Postiz analytics (if using Postiz) / native TikTok stats (manual entry ok).
- Airtable `update_records_for_table`, `Run Log`.

## Procedure
1. For each `Posted` row, pull available metrics (some may be manual entry from
   TikTok/affiliate dashboards — link clicks & conversions often live there).
2. Fill the metric fields; note standout winners/losers.
3. Summarize for the human: top performers, weak angles, category trends.
4. Recommend next Scout focus (double down on what worked).

## Guardrails
- Be honest about data gaps (e.g. affiliate conversions may be dashboard-only).
- Don't overclaim causation from small samples; flag when N is tiny.
