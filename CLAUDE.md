# TempleOTemple — Build Instructions for Claude Code

> **Read this first.** You are helping build **TempleOTemple**, a temple-commerce
> super app for India (Tamil Nadu). This file + the `docs/` folder are the complete
> spec. The **first task is the admin/vendor dashboard.**

## What we're building
A two-sided marketplace + a flagship temple-crowdfunding platform:
- **Buyer app** (Android + iOS) — devotees browse, order, book, pay
- **Seller app** (Android + iOS) — vendors list, manage orders/bookings, get paid
- **Web dashboard** (admin + vendor) — ← **BUILD THIS FIRST**
- **Build-a-Temple crowdfunding** — GoFundMe-style campaigns (flagship)

## Tech stack (do not deviate without updating docs)
| Part | Language | Framework |
|---|---|---|
| **Web dashboard** (first) | TypeScript | Next.js + Tailwind CSS |
| Mobile apps | Dart | Flutter |
| Database | SQL | Supabase (PostgreSQL + RLS) |
| Backend functions | TypeScript | Supabase Edge Functions (Deno) |
| Payments | — | Razorpay (Checkout + Route split payout) |
| Hosting | — | Dashboard → Vercel; static/CDN/images → Cloudflare |

## Locked decisions
- Market: **India (Tamil Nadu)**, currency **INR** (store money as **paise, BIGINT**)
- Language: **English default**, Tamil toggle in settings (no launch picker)
- Auth: **guest browsing; OTP login only at purchase**
- Revenue: **commission per order** (category-wise %), Razorpay Route auto-split payout
- Base: this project is a **fork/adaptation of the `cuckoo` marketplace** — reuse its
  payment, GST, category engine, and KYC patterns; do not rebuild them from scratch
- General donations are **deferred**; Build-a-Temple crowdfunding is the flagship

## The spec — read these before coding
- `docs/MASTER_PLAN.md` — decisions, stack, phases, review gate (start here)
- `docs/FEATURE_MATRIX.md` — every feature → phase → status
- `docs/DATA_MODEL.md` — Supabase tables, RLS, atomic functions
- `docs/CATEGORY_TREE.md` — 7 verticals → `categories` table
- `docs/WORKFLOWS.md` — 16 business workflows (orders, payouts, crowdfunding)
- `docs/BuildMaster.html` / `docs/Proposal.html` — human-readable overview + mockups

## FIRST TASK — Admin/Vendor Dashboard (Phase 1)

Build a **Next.js + TypeScript + Tailwind** dashboard backed by Supabase. Deliver these
screens (see `docs/FEATURE_MATRIX.md` §13 and `docs/WORKFLOWS.md`):

1. **Auth & layout** — Supabase Auth login, role-based routing (super_admin, temple_admin, vendor_admin), sidebar nav
2. **Dashboard home** — summary cards: today's orders, active bookings, vendor revenue, active vendors
3. **Vendor approvals** — list pending vendors, view KYC (GSTIN/PAN/bank), approve/reject
4. **Orders** — list/filter orders, statuses (pending→paid→packed→shipped→delivered→refunded)
5. **Payouts** — per-vendor payout queue, commission retained, settlement status
6. **Commission settings** — commission % per category (writes to `platform_settings`)
7. **Build-a-Temple campaigns** — list campaigns, goal/raised/progress, verify milestone → release funds
8. **Reports** — revenue, GMV, orders by category

### Suggested order of work
1. Create a Supabase project; apply the schema from `docs/DATA_MODEL.md` as migrations (money = paise BIGINT, RLS on every table).
2. Seed `categories` from `docs/CATEGORY_TREE.md` (English now; `name_ta` later), with `vendor_type` + `fulfillment_type`.
3. Scaffold the Next.js app (`npx create-next-app`, TypeScript + Tailwind) OR port cuckoo's `admin-dashboard/` if available.
4. Wire Supabase client + Auth + role routing.
5. Build the screens above, in order.
6. Before calling it done, run the review gate in `docs/MASTER_PLAN.md` §5.

## Conventions
- Money: integer **paise**; never floats for currency.
- Every table: **RLS** (buyers own rows, vendors own rows, temple admins own temple).
- Localization: English default + Tamil strings; no hardcoded user-facing text.
- Keep `docs/` as the source of truth — if a decision changes, update the doc first.
