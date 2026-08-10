# Reel Engine — TikTok + IG Affiliate Agent Stack

A packaged, **human-in-the-loop** content engine that turns trending
health / wellness / fitness / women-niche affiliate products into published
**TikTok and Instagram** short-form videos — with a human approval gate at every
stage, orchestrated by **Hermes**.

Niche focus: **women's health, wellness & fitness.**
Ships in three forms (see [`docs/PRODUCT.md`](docs/PRODUCT.md)):
**① packaged repo · ② hosted Base44 app · ③ productized service kit.**
Built **multi-tenant** — run it for your own brand or resell it to clients.

---

## The Lineup — 6 agents, 4 human gates, 1 orchestrator

```
   HERMES  (orchestrator / runtime — runs agents, drives the state machine,
            schedules cycles, routes the 4 gate notifications to the human)
   ────────────────────────────────────────────────────────────────────────
                 ┌──────────────────────────────────────────────┐
                 │      AIRTABLE = control panel / state store    │
                 └──────────────────────────────────────────────┘

 ⓪ RESEARCHER ─▶ ① SCOUT ─▶ 🛑G1 ─▶ ② DIRECTOR ─▶ 🛑G2 ─▶ ③ COPYWRITER ─▶ 🛑G3 ─▶ ④ PUBLISHER ─▶ 🛑G4 ─▶ LIVE ─▶ ⑤ ANALYST
   trend &        product     pick    Higgsfield     clip    caption + IG      copy    Postiz →         final    TikTok
   audience       picks +      prod.   clips (img2vid  ok      short copy +      ok      TikTok + IG      go       + IG
   intel          links               / UGC) + Canva          link + #ad                + Canva covers
```

| # | Agent | Does | Key tools | Status it writes |
|---|-------|------|-----------|------------------|
| ⓪ | **Researcher** | Trend/audience/sound intel → briefs | WebSearch, Higgsfield `tiktok_music_trending`, `virality_predictor` | `Trend Briefs: New` |
| ① | **Scout** | In-niche product picks from briefs | WebSearch / WebFetch | `Proposed` |
| ② | **Director** | Product → 9:16 clip + Canva cover | Higgsfield (img2vid / UGC), Canva | `Clip Ready` |
| ③ | **Copywriter** | TikTok caption **+ IG short copy** + link + `#ad` | LLM | `Copy Ready` |
| ④ | **Publisher** | Post to **TikTok + IG** | Postiz, Canva | `Scheduled`→`Posted` |
| ⑤ | **Analyst** | Views / clicks / conversions | Higgsfield virality, platform stats | metrics |

### The four human check-in gates
1. **🛑 Product selection** — Scout proposes; you pick + paste the affiliate link.
2. **🛑 Clip approval** — approve each Higgsfield clip (and Canva cover).
3. **🛑 Caption + link approval** — approve copy, hashtags, link, `#ad` disclosure.
4. **🛑 Before it posts** — final go/no-go before TikTok/IG publish.

Each gate = a Status change in Airtable. Hermes pings you; nothing crosses a 🛑
gate without your yes. Full cycle: [`docs/RUNBOOK.md`](docs/RUNBOOK.md).

---

## What's connected (reality check)
- ✅ **Hermes** — orchestrator/runtime (drives the stack). Adapter: [`docs/ORCHESTRATION_HERMES.md`](docs/ORCHESTRATION_HERMES.md).
- ✅ **Higgsfield** — image + video generation, virality, trending sounds.
- ✅ **Postiz** — schedules/posts to **TikTok + Instagram** (+ links / short links).
- ✅ **Canva** — covers, text-overlays, carousels, thumbnails.
- ✅ **Airtable** — control panel / state machine / approvals.
- ✅ **Base44** — hosts the optional clickable app (form ②).
- ⚠️ **No TikTok Shop affiliate API** — Scout finds products; **you supply the
  affiliate link** at Gate 1.
- 🔐 **Connect a TikTok + IG account** in Postiz before publishing. See [`docs/SETUP.md`](docs/SETUP.md).

> ⚖️ Every post carries `#ad` / affiliate disclosure and follows FTC + platform
> affiliate rules. Enforced at Copywriter, re-checked at Gate 3.

---

## Repo map
```
README.md                      ← the lineup (this file)
docs/
  ARCHITECTURE.md              ← data flow, state machine, tool map
  ORCHESTRATION_HERMES.md      ← how Hermes runs the stack (runtime contract)
  RUNBOOK.md                   ← the operating cycle + 4 gates
  AIRTABLE_SCHEMA.md           ← control-panel data model (multi-tenant)
  PRODUCT.md                   ← packaging: 3 ship forms, pricing, onboarding
  MULTI_TENANT.md              ← running it per-client (agency mode)
  SETUP.md                     ← one-time connection checklist
config/
  niche.md                     ← niche, voice, banned claims, hashtags
orchestration/
  stack.yaml                   ← agent + gate manifest Hermes runs
agents/
  0-researcher.md  1-scout.md  2-director.md
  3-copywriter.md  4-publisher.md  5-analyst.md
content/
  ig-viral-scripts.md          ← 3 ready-to-shoot IG short-video scripts
```

## Quick start
1. Connect accounts → [`docs/SETUP.md`](docs/SETUP.md).
2. Stand up the Airtable base → [`docs/AIRTABLE_SCHEMA.md`](docs/AIRTABLE_SCHEMA.md).
3. Point Hermes at [`orchestration/stack.yaml`](orchestration/stack.yaml).
4. Run your first cycle → [`docs/RUNBOOK.md`](docs/RUNBOOK.md).
5. Selling it? → [`docs/PRODUCT.md`](docs/PRODUCT.md).
