# Architecture

## Design principle

**Airtable is the single source of truth.** Every product is one row. Its
`Status` field is a state machine that moves left-to-right through the pipeline.
Agents only ever act on rows in the status they own, and they hand off by
changing the status. Humans approve by changing the status too (or via the
Airtable interface buttons). This means:

- The system is **resumable** — pick up any row at any stage.
- The system is **auditable** — every product's history lives in one row.
- **Nothing skips a gate** — an agent literally has no rows to act on until the
  human moves them into its "inbox" status.

## State machine

```
Proposed ──(GATE 1: you approve + add link)──▶ Approved
Approved ──(Director builds clip)────────────▶ Clip Ready
Clip Ready ──(GATE 2: you approve clip)───────▶ Clip Approved
Clip Approved ──(Copywriter writes)───────────▶ Copy Ready
Copy Ready ──(GATE 3: you approve caption)─────▶ Copy Approved
Copy Approved ──(Publisher schedules)──────────▶ Scheduled
Scheduled ──(GATE 4: final go / it posts)──────▶ Posted
Posted ──(Analyst)─────────────────────────────▶ (metrics filled in)

Any stage ──(you reject)──▶ Rejected  (with a reason, exits the pipeline)
```

Each agent's "inbox" = rows in the status just before it. Each agent's job =
do its work, fill its fields, advance the status to the next "waiting for human"
state. It never advances past a gate.

## Data flow per product

```
Scout          →  writes: Product Name, Why Trending, Source URL, Category,
                           Suggested Angle          | Status: Proposed
── GATE 1 (human) → adds: Affiliate Link, (optional) Product Image
                           | Status: Approved
Director       →  writes: Clip Format (image2video|ugc), Higgsfield Job ID,
                           Clip URL                  | Status: Clip Ready
── GATE 2 (human) → Status: Clip Approved   (or Rejected → back to Director)
Copywriter     →  writes: Hook, Caption, Hashtags, Disclosure ✓, Final Caption
                           | Status: Copy Ready
── GATE 3 (human) → Status: Copy Approved
Publisher      →  writes: Scheduled Time, Platform Post ID, Post Status
                           | Status: Scheduled → Posted
── GATE 4 (human) → the go/no-go on the scheduled/queued post
Analyst        →  writes: Views, Likes, Clicks, Est. Conversions, Notes
```

## Tool reality-check (what's actually wired up)

| Capability | Tool | Status |
|------------|------|--------|
| Product discovery | `WebSearch` / `WebFetch` (Scout does research) | ✅ available |
| ~~TikTok Shop affiliate feed~~ | — | ❌ **no API** — human supplies links |
| Product image → vertical clip | Higgsfield `generate_video` (image2video) | ✅ |
| UGC / creator-style ad | Higgsfield Marketing Studio / `shorts_studio` | ✅ |
| Trending audio | Higgsfield `tiktok_music_trending` | ✅ |
| Virality prediction | Higgsfield `virality_predictor` | ✅ |
| Publish to TikTok | Higgsfield `tiktok_publish` **or** Postiz | ✅ (needs account connected) |
| Schedule across platforms | Postiz `integrationSchedulePostTool` | ✅ |
| Control panel / approvals | Airtable | ✅ |

### Two clip paths (you chose "decide per product")

The Director picks per product:

- **Path A — Image→Video** (`Clip Format = image2video`): you/Scout provide a
  real product photo; Higgsfield animates it into a 9:16 clip. Most faithful to
  the actual product. Best when the physical product is the star (supplements,
  fitness gear, beauty tools).
- **Path B — UGC ad** (`Clip Format = ugc`): Higgsfield Marketing Studio
  generates a creator-style talking/demo clip from the product info. Best for
  lifestyle / benefit-led angles where a "person using it" sells better.

The Director recommends a path in the row; you can override at Gate 2.

## Why human-in-the-loop, mechanically

Agents are **pull-based on status**, so a human gate is simply *"the row sits in
status X until a person moves it to X+1."* No agent polls past its own inbox.
That's what makes the four gates real rather than advisory.

## Orchestration options (how the agents actually run)

Pick based on how hands-on you want to be — all use the same Airtable state machine:

1. **Chat-driven (default, simplest):** you tell me "run Scout" / "run the
   Director on the approved rows" and I execute that stage, then stop at the
   gate. Zero infra. Recommended to start.
2. **Scheduled:** a recurring job runs Scout weekly and pings you at each gate.
   (Cron / `/loop`.) Add once the manual cycle feels good.
3. **Airtable-button driven:** Airtable interface buttons flip statuses; you
   work entirely from the board. Nice once volume grows.

See [`RUNBOOK.md`](RUNBOOK.md) for the concrete cycle.
