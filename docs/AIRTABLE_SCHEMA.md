# Airtable Control-Panel Schema

Base name: **TikTok Affiliate Engine**

This is the control panel. One base, one primary table (`Products`) that is the
state machine, plus two small support tables.

## Table 1 — `Products` (the pipeline board)

| Field | Type | Written by | Notes |
|-------|------|-----------|-------|
| **Product Name** | Single line text | Scout | Primary field |
| **Status** | Single select | everyone | The state machine — options below |
| **Category** | Single select | Scout | Wellness / Fitness / Beauty / Supplements / Women's Health / Other |
| **Why Trending** | Long text | Scout | Evidence + why it fits the niche |
| **Source URL** | URL | Scout | Where Scout found it |
| **Suggested Angle** | Long text | Scout | Content angle idea |
| **Affiliate Link** | URL | 🧑 **You (Gate 1)** | The real TikTok/affiliate link |
| **Product Image** | Attachment | You / Scout | Needed for image→video path |
| **Clip Format** | Single select | Director | `image2video` / `ugc` |
| **Higgsfield Job ID** | Single line text | Director | For traceability |
| **Clip URL** | URL | Director | The generated 9:16 clip |
| **Hook** | Single line text | Copywriter | First 1–2 seconds line |
| **Caption** | Long text | Copywriter | Body |
| **Hashtags** | Long text | Copywriter | Space-separated |
| **Disclosure OK** | Checkbox | Copywriter | `#ad` / affiliate disclosure present |
| **Final Caption** | Long text | Copywriter | Caption + hashtags + link, ready to post |
| **Scheduled Time** | Date/time | Publisher | UTC |
| **Platform Post ID** | Single line text | Publisher | Postiz/TikTok post id |
| **Post Status** | Single select | Publisher | draft / scheduled / posted / failed |
| **Views** | Number | Analyst | |
| **Likes** | Number | Analyst | |
| **Link Clicks** | Number | Analyst | |
| **Est. Conversions** | Number | Analyst | |
| **Reject Reason** | Long text | You | Why a row was rejected |
| **Owner** | Single line text | — | Who's handling (you / VA) |

### `Status` single-select options (the state machine)

Order + suggested colors:

1. `Proposed` — gray *(Scout's output; Gate 1 inbox)*
2. `Approved` — blue *(Director inbox)*
3. `Clip Ready` — cyan *(Gate 2 inbox)*
4. `Clip Approved` — teal *(Copywriter inbox)*
5. `Copy Ready` — yellow *(Gate 3 inbox)*
6. `Copy Approved` — orange *(Publisher inbox)*
7. `Scheduled` — purple *(Gate 4 inbox)*
8. `Posted` — green *(Analyst inbox / done)*
9. `Rejected` — red *(exited)*

> The four human gates are simply: rows sitting in `Proposed`, `Clip Ready`,
> `Copy Ready`, and `Scheduled` are waiting on **you**.

## Table 2 — `Niche Rules` (guardrails the agents read)

Single-row-ish reference table so agents share one definition of the niche.

| Field | Type | Example |
|-------|------|---------|
| Rule Name | Single line text | "Banned claims" |
| Detail | Long text | "No 'cures', 'guaranteed weight loss', medical claims…" |
| Applies To | Multiple select | Scout / Copywriter / Publisher |

Seeded from [`config/niche.md`](../config/niche.md).

## Table 3 — `Run Log` (audit trail)

| Field | Type | Notes |
|-------|------|-------|
| Timestamp | Created time | |
| Agent | Single select | Scout / Director / Copywriter / Publisher / Analyst |
| Product | Link to `Products` | |
| Action | Long text | What happened |
| Result | Single select | ok / needs-human / error |

## Recommended views on `Products`

- **① Approve products** → filter `Status = Proposed`
- **② Approve clips** → filter `Status = Clip Ready`
- **③ Approve captions** → filter `Status = Copy Ready`
- **④ Approve to post** → filter `Status = Scheduled`
- **Live** → filter `Status = Posted`
- **Rejected** → filter `Status = Rejected`

Each view = one of your check-in queues.
