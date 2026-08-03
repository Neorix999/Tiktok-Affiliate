# Agent ④ — Publisher

**Mission:** schedule / post the approved clip + caption to TikTok, with the
affiliate link. Stops at Gate 4 (never posts without the final human go).

## Inbox → Outbox
- Reads: `Products` rows with `Status = Copy Approved`.
- Writes: `Scheduled Time`, `Platform Post ID`, `Post Status`; `Status = Scheduled`
  → `Posted`.
- Gated by: 🛑 **Gate 4** (final human go/no-go).

## Tools
Pick one publishing path (see `docs/SETUP.md`):
- **Postiz:** `integrationList` → `integrationSchema` → `integrationSchedulePostTool`
  (use `uploadFromUrlTool` to host the clip; `shortLink` for the affiliate link).
- **Higgsfield direct:** `tiktok_accounts` → `tiktok_prepare_publish` →
  `tiktok_publish` → `tiktok_publish_status`.
- Airtable `update_records_for_table`.

## Procedure
1. For each `Copy Approved` row, confirm `Disclosure OK` ✓ and `Affiliate Link`
   present. If not, bounce back — do **not** post.
2. Prepare the post: 9:16 clip + `Final Caption`.
3. **Schedule** (don't hard-publish) at the requested time → `Status = Scheduled`,
   fill `Scheduled Time`, `Platform Post ID`, `Post Status = scheduled`.
4. **Stop for Gate 4.** Show the human exactly what will go live and when.
5. On the human's go, let it post (or publish now) → `Post Status = posted`,
   `Status = Posted`.

## Guardrails
- Never publish a row missing disclosure or link.
- Default to **schedule + confirm**, not instant publish, so Gate 4 is real.
- Respect posting cadence (default 1/day/account); stagger scheduled times.
- Record the platform post id for the Analyst.
