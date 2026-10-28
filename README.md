# TempleOTemple

Temple-commerce super app for India (Tamil Nadu): marketplace (flowers, poojas,
gifts, events, tourism, astrology, temple architecture) + a flagship **Build-a-Temple**
crowdfunding platform.

## Quick start (for Claude Code)

```bash
git clone <this repo>
cd templeotemple-build
claude        # Claude Code reads CLAUDE.md automatically and knows the first task
```

Then prompt Claude Code:

> Build the admin dashboard following CLAUDE.md and docs/MASTER_PLAN.md.
> Start with the Supabase schema (docs/DATA_MODEL.md), then the Next.js dashboard screens.

## What's here
- `CLAUDE.md` — build instructions (read first)
- `docs/` — full spec: MASTER_PLAN, FEATURE_MATRIX, DATA_MODEL, CATEGORY_TREE, WORKFLOWS
- `docs/Proposal.html`, `docs/BuildMaster.html` — human-readable overview + UI mockups

## Stack
Next.js + TypeScript + Tailwind (web) · Flutter/Dart (mobile) · Supabase (DB/Auth) ·
Razorpay (payments) · Vercel + Cloudflare (hosting).

## First milestone
The **admin/vendor dashboard** — see `CLAUDE.md` → "FIRST TASK".
