# TikTok Affiliate Agent System

An orchestrated, **human-in-the-loop** content engine that turns trending
health / wellness / fitness / women-niche affiliate products into published
TikTok clips — with a human approval gate at every stage.

Niche focus: **women's health, wellness & fitness.**

---

## The Lineup

Five agents run as a pipeline. Between each stage there is a **🛑 human
check-in gate** — nothing advances until you approve.

```
                 ┌─────────────────────────────────────────────────────────┐
                 │              AIRTABLE  =  the control panel               │
                 │   every product is one row; its Status drives the flow    │
                 └─────────────────────────────────────────────────────────┘

  ①  SCOUT ─────────▶ 🛑 GATE 1 ─────▶ ②  DIRECTOR ─────▶ 🛑 GATE 2 ─────▶
  web-researches       Product          builds the clip     Clip
  trending picks       selection        (Higgsfield)        approval
  (health/fitness      + you add
   /women niche)       affiliate link

  ③  COPYWRITER ────▶ 🛑 GATE 3 ─────▶ ④  PUBLISHER ────▶ 🛑 GATE 4 ─────▶  LIVE
  hook + caption       Caption &        schedules to        Final
  + hashtags +         link             TikTok              approval
  affiliate link +     approval         (Postiz / Higgsfield) before it posts
  #ad disclosure

  ⑤  ANALYST  (optional, after posting) — pulls performance, feeds next cycle.
```

| # | Agent | Does | Tools | Writes status |
|---|-------|------|-------|---------------|
| ① | **Scout** | Finds trending in-niche products, proposes candidates | WebSearch / WebFetch | `Proposed` |
| ② | **Director** | Turns the approved product into a vertical clip | Higgsfield (image→video **or** UGC ad) | `Clip Ready` |
| ③ | **Copywriter** | Writes hook, caption, hashtags, inserts affiliate link + disclosure | (LLM) | `Copy Ready` |
| ④ | **Publisher** | Schedules / posts the clip with the link | Postiz + Higgsfield TikTok | `Scheduled` |
| ⑤ | **Analyst** | Reports views / clicks / conversions | Higgsfield virality, TikTok analytics | `Posted` → metrics |

---

## The four human check-in gates

You asked for a check-in at every hand-off. Here they are:

1. **🛑 Product selection** — Scout proposes; you pick which products advance
   and paste in the real affiliate link. *(Nothing gets produced without your
   yes.)*
2. **🛑 Clip approval** — you watch each Higgsfield clip before any copy is written.
3. **🛑 Caption + link approval** — you approve the wording, hashtags, and that
   the affiliate link + `#ad` disclosure are correct.
4. **🛑 Before it posts** — final go/no-go; the Publisher only schedules or goes
   live after your last yes.

Each gate is just a Status change in Airtable (or a "yes" to me in chat). See
[`docs/RUNBOOK.md`](docs/RUNBOOK.md) for the exact click-by-click cycle.

---

## Repo map

```
README.md                  ← you are here (the lineup)
docs/
  ARCHITECTURE.md          ← how the pieces fit, data flow, tool reality-check
  RUNBOOK.md               ← the operating cycle + the 4 gates, step by step
  AIRTABLE_SCHEMA.md       ← the control-panel data model (tables/fields/status)
  SETUP.md                 ← one-time connection checklist (what needs your OAuth)
config/
  niche.md                 ← the niche definition, guardrails, brand voice
agents/
  1-scout.md               ← Scout agent spec + prompt
  2-director.md            ← Director agent spec + prompt
  3-copywriter.md          ← Copywriter agent spec + prompt
  4-publisher.md           ← Publisher agent spec + prompt
  5-analyst.md             ← Analyst agent spec + prompt
```

---

## Reality check (read this first)

- ✅ **Higgsfield** is connected — image + video generation, and it can publish
  directly to TikTok (`tiktok_*` tools).
- ✅ **Postiz** is connected — schedules posts to TikTok and other platforms,
  supports links + short links.
- ✅ **Airtable** is connected — used as the control panel / approval board.
- ⚠️ **There is no direct TikTok Shop affiliate API.** Products are found by the
  **Scout** agent via web research; **you supply the actual affiliate link** at
  Gate 1. This is a deliberate design choice, not a limitation we can code away.
- 🔐 **You must connect a TikTok account** (in Higgsfield and/or Postiz) before
  the Publisher can post. See [`docs/SETUP.md`](docs/SETUP.md).

> ⚖️ **Compliance:** every post must carry an affiliate/ad disclosure (e.g.
> `#ad`) and follow TikTok's affiliate + FTC rules. This is enforced at the
> Copywriter stage and re-checked at Gate 3.
