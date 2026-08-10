# One-time setup checklist

Do these once. Items marked 🔐 need **you** to authorize (OAuth / connect an
account) — I can't do those in a non-interactive session.

## 1. Airtable control panel
- [ ] I create the **TikTok Affiliate Engine** base with the tables/fields in
      [`AIRTABLE_SCHEMA.md`](AIRTABLE_SCHEMA.md).
- [ ] I seed the `Niche Rules` table from [`config/niche.md`](../config/niche.md).
- [ ] I create the six approval **views** (one per gate).

*(This runs through the Airtable MCP — no action needed from you beyond it being
connected, which it is.)*

## 2. TikTok posting account  🔐
Pick at least one publishing path:

Publishing runs on **two separate rails** — they use different tools:

**Instagram rail — Postiz**
- [ ] In Postiz, connect **Instagram**. Confirm it appears in `integrationList`.

**TikTok rail — Higgsfield ONLY**
- [ ] Connect your TikTok account via Higgsfield (`tiktok_connect`).
- [ ] Confirm it shows as `active` (`tiktok_accounts`).
- [ ] (Fallback) Or plan to export the clip and upload to TikTok manually.

> ❌ **Postiz cannot post to TikTok on this stack.** Do not try to connect TikTok
> in Postiz for publishing. IG → Postiz, TikTok → Higgsfield (or manual). They are
> independent lanes sharing the same approved clip + copy.

## 3. Higgsfield credits
- [ ] Confirm you have generation credits (`balance` / `show_plans_and_credits`).
      Each clip consumes credits; the Director previews cost with `get_cost`
      before generating so there are no surprises.

## 4. Niche & brand voice
- [ ] Review [`config/niche.md`](../config/niche.md) and tweak the voice,
      banned claims, and target sub-topics to your brand.

## 5. Affiliate links  🔐
- [ ] Have your TikTok affiliate / creator-marketplace links ready. You paste the
      correct link per product at **Gate 1**. (No API pulls these automatically.)

## Not connected / needs auth right now
- **Canva**, **META** — require authorization before use. Not needed for the core
  pipeline; connect later only if you want Canva graphics or Meta ads.

---

When 1–4 are done, you're ready to run the first cycle — see
[`RUNBOOK.md`](RUNBOOK.md).
