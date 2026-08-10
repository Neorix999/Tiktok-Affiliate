# Product packaging — ship it today

Reel Engine ships in **three forms**. You can package all three from this repo.

```
① PACKAGED REPO ──────▶ hand to a dev / run yourself. Source of truth.
② HOSTED APP (Base44) ─▶ clickable dashboard + approval buttons for non-technical operators.
③ SERVICE KIT ────────▶ done-for-you offer you sell to clients (agency mode).
```

All three share the **same brain**: the agent specs, `stack.yaml`, and the
Airtable control panel. The app and the service are packaging on top.

---

## ① Packaged repo (the product core)
Everything in this repo. A buyer/operator gets: the 6 agent specs, the Hermes
manifest, the Airtable schema, the runbook, and the niche config. Deploy by
connecting accounts ([`SETUP.md`](SETUP.md)) and pointing Hermes at
[`../orchestration/stack.yaml`](../orchestration/stack.yaml).

**Sell as:** a template / license, or the deliverable inside a service engagement.

## ② Hosted app (Base44)
A clickable front end so a non-technical operator lives in one dashboard instead
of raw Airtable:
- **Pipeline board** — products moving through the 6 stages.
- **4 gate screens** — one-tap Approve / Reject at each check-in, with the clip
  + caption shown inline.
- **New cycle** button → kicks Researcher + Scout.
- **Analytics** — the Analyst's numbers.

Backed by the same Airtable data. Build target: Base44 (connected). This is the
"feels like a real SaaS" layer.

## ③ Productized service kit (agency mode)
Sell it done-for-you. What the client gets, and a starting price ladder:

| Tier | What they get | Suggested price |
|------|---------------|-----------------|
| **Starter** | 12 videos/mo (TikTok **or** IG), 1 niche, weekly Researcher cycle, all 4 gates via a shared Airtable | $X/mo |
| **Growth** | 30 videos/mo, TikTok **+** IG cross-post, Canva covers, trending-sound matching, analytics review | $XX/mo |
| **Agency/White-label** | Multi-brand, multi-tenant, your logo on the Base44 app, priority cycles, monthly strategy report | $XXX/mo |

**Onboarding (repeatable, ~30 min/client):**
1. Client connects TikTok + IG in Postiz; shares affiliate-link source.
2. You clone the Airtable base for their `Tenant/Client` (see [`MULTI_TENANT.md`](MULTI_TENANT.md)).
3. Tune `config/niche.md` to their brand voice + banned claims.
4. Run a first Researcher→Scout cycle; client approves at Gate 1.
5. Set posting cadence; go live.

**Deliverable each cycle:** approved, disclosed, scheduled short-form videos on
the client's accounts — with them approving at every gate, so they stay in control
and on-brand.

---

## Positioning one-liner
> *"An AI content team for wellness affiliate brands — trend research, video
> production, copy, and posting across TikTok + Instagram — with a human approval
> at every step, so it never posts anything you didn't sign off on."*

## What makes it defensible
- **Human-in-the-loop by architecture** (not bolt-on) → brand-safe, FTC-compliant.
- **Niche-tuned** (women's wellness) → sharper picks and voice than a generic tool.
- **Multi-tenant from day one** → resell without re-plumbing.
- **Trend-native** → rides current sounds/formats, not evergreen filler.

## Ship-today checklist
- [x] Repo packaged (agents, manifest, schema, runbook, niche).
- [ ] Airtable base created + seeded (I can do this now).
- [ ] Base44 app scaffolded (I can kick this off).
- [ ] Accounts connected (🔐 you: TikTok + IG in Postiz).
- [ ] First cycle run through Gate 1.
