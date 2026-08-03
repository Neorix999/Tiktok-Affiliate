# Runbook — the operating cycle

This is the repeatable loop. One "cycle" takes a batch of products from idea to
posted. You touch it at four gates; agents do everything else.

## The cycle at a glance

```
 YOU: "run a scout cycle"                      ← kick off
   │
   ▼
 ① SCOUT researches → drops N candidates in Airtable as `Proposed`
   │
   ▼
🛑 GATE 1 — you open the "① Approve products" view:
   • keep the ones you like → paste Affiliate Link → set Status `Approved`
   • kill the rest → Status `Rejected` (+ reason)
   │
   ▼
 ② DIRECTOR builds a 9:16 clip for each `Approved` row (Higgsfield),
    picks image2video or ugc → Status `Clip Ready`
   │
   ▼
🛑 GATE 2 — you open "② Approve clips": watch each clip
   • good → Status `Clip Approved`
   • redo → Status back to `Approved` with a note (Director regenerates)
   │
   ▼
 ③ COPYWRITER writes hook + caption + hashtags + link + #ad → Status `Copy Ready`
   │
   ▼
🛑 GATE 3 — you open "③ Approve captions": check wording, link, disclosure
   • good → Status `Copy Approved`
   • tweak → edit inline or bounce back with a note
   │
   ▼
 ④ PUBLISHER schedules the post (Postiz/Higgsfield TikTok) → Status `Scheduled`
   │
   ▼
🛑 GATE 4 — you open "④ Approve to post": final go/no-go
   • go → it posts at the scheduled time → Status `Posted`
   • hold → Status back / unschedule
   │
   ▼
 ⑤ ANALYST (later) fills Views/Likes/Clicks → informs next Scout cycle
```

## Running it chat-driven (default)

You don't need any automation to start. Just tell me the stage:

- **"Run a scout cycle for [sub-topic], N products."** → I run Scout, write rows,
  and stop. I'll summarize the candidates for you.
- *You approve in Airtable (Gate 1) and add links.*
- **"Build the clips."** → I run the Director on `Approved` rows and stop at
  Gate 2 with links to each clip.
- *You approve clips (Gate 2).*
- **"Write the copy."** → Copywriter fills captions, stops at Gate 3.
- *You approve captions (Gate 3).*
- **"Schedule them for [time]."** → Publisher schedules, stops at Gate 4.
- *You give the final go (Gate 4).*

At every hand-off I **stop and wait for you** — that's the human-in-the-loop
guarantee. I never cross a 🛑 gate on my own.

## What I will always show you at each gate

- **Gate 1:** product name, why it's trending, source link, suggested angle — so
  you can decide fast and drop in the affiliate link.
- **Gate 2:** a direct link/preview to each generated clip + which format was used.
- **Gate 3:** the exact final caption (hook + body + hashtags + affiliate link +
  `#ad`), with the disclosure checkbox state called out.
- **Gate 4:** the scheduled time, target account, and the clip+caption together,
  as it will appear live.

## Cadence suggestion

- **Scout:** weekly (or when you want fresh picks).
- **Batch size:** start with **3 products/cycle** to calibrate quality before
  scaling.
- **Posting:** 1 post/day/account is a safe starting rhythm; the Publisher can
  stagger scheduled times.

## Rejection / redo paths

- Reject a product → `Rejected` + `Reject Reason`. It leaves the pipeline.
- Bad clip → set back to `Approved`; Director regenerates (optionally with a new
  angle/format note).
- Weak caption → edit inline at Gate 3, or bounce to `Clip Approved` for a rewrite.

## Compliance checklist (enforced at Gate 3)

- [ ] `#ad` (or clear affiliate disclosure) present in the caption.
- [ ] No prohibited health claims (no "cures", "guaranteed results", medical
      claims — see `config/niche.md`).
- [ ] Affiliate link is the correct one for *this* product.
- [ ] Clip has no unlicensed music/brand issues (prefer Higgsfield trending audio).
