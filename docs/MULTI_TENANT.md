# Multi-tenant (agency mode)

Run one engine for many brands/clients without their data or posts mixing.

## The model
Every record carries a **`Tenant/Client`** field. All agents, views, and Hermes
jobs filter by it. One Airtable base can hold many clients, *or* you clone the
base per client for hard isolation.

| Approach | Isolation | When |
|----------|-----------|------|
| **Shared base + `Tenant` field** | Logical | You run a few brands; simplest. |
| **Base-per-client** | Physical | Client wants their own data / you white-label. |

## What's scoped per tenant
- **Airtable:** `Tenant/Client` on every row; views filtered per client.
- **Config:** a niche/voice profile per client (copy `config/niche.md` →
  `config/clients/<client>.md`).
- **Accounts:** each client connects their own TikTok + IG in Postiz; Publisher
  targets that client's `integrationId`.
- **Affiliate links:** per client (their programs, their commissions).
- **Hermes:** fan-out — the same `stack.yaml` runs per tenant with the tenant id
  bound; state stays isolated by filter/base.

## Onboarding a new client (repeatable)
1. Add the client to `Tenant/Client` (or clone the base).
2. Create `config/clients/<client>.md` from the niche template; tune voice +
   banned claims.
3. Client connects TikTok + IG in Postiz; you note the `integrationId`s.
4. Collect their affiliate-link source.
5. Kick a Researcher→Scout cycle scoped to that tenant; client approves Gate 1.

## Guardrails
- Never cross-post one client's content to another's accounts — Publisher must
  match `Tenant/Client` → `integrationId`.
- Keep per-client compliance rules in that client's config (some brands have
  stricter claim rules).
- Analyst reports are per-tenant.
