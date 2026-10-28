# TempleOTemple — Master Plan

> Unified Temple Commerce Super App. **Base strategy: fork `cuckoo`** (mature
> India-bound marketplace) and adapt for temple commerce.

> **Two masters, two jobs:**
> - 📣 **Pitch master** (for Bala): `Proposal.html`
> - 🛠️ **Build master** (for engineering): this file + the doc set below
>
> **Build master doc set:**
> - [FEATURE_MATRIX.md](FEATURE_MATRIX.md) — every feature → phase / screen / status
> - [CATEGORY_TREE.md](CATEGORY_TREE.md) — 7-vertical menu → categories table
> - [DATA_MODEL.md](DATA_MODEL.md) — Supabase schema
> - [WORKFLOWS.md](WORKFLOWS.md) — all business workflows (happy + failure + back-office)

---

## 1. Locked decisions

| Decision      | Choice                          | Consequence                                       |
|---------------|---------------------------------|---------------------------------------------------|
| Market        | **India (Tamil Nadu)**          | Razorpay, INR                                     |
| Language      | **English primary, Tamil optional** | No language picker on launch; Tamil toggle in settings |
| Auth          | **Login only at purchase**      | Guest browsing/search; OTP login triggered at checkout |
| Revenue       | **Commission per order**        | Category-wise % + Razorpay Route split payout     |
| MVP scope     | **All 7 modules** (phased)      | Product flow + service-booking flow both day 1    |
| Flagship      | **Build-a-Temple crowdfunding** | GoFundMe-style temple projects for revered saints/gurus |
| Donations     | **Deferred**                    | General temple/annadhanam donations moved to a later phase |
| Pay / fulfill | **Full prepay + auto split**    | Razorpay Route; vendor auto-paid minus commission |
| Base codebase | **Fork cuckoo**                 | Inherit payments, GST, category engine, KYC, store|

---

## 2. Stack (inherited from cuckoo)

- **Mobile**: Flutter — buyer app + seller app + shared `temple_common` package
- **Web**: Next.js admin dashboard (Vercel / Cloudflare Pages)
- **Backend**: Supabase (Postgres + RLS + Realtime + Auth + Storage + Edge Functions)
- **Payments**: Razorpay (checkout + Route split settlement + Subscriptions e-mandate)
- **Push**: Firebase FCM
- **Media**: Cloudflare Stream (video), Supabase/R2 (images)
- **Maps**: Google Maps / MapTiler
- **AI**: Claude API (temple finder, recommendations, planner)
- **Shipping**: Shiprocket aggregator (pincode zones, COD serviceability)

---

## 3. What cuckoo already gives us (reuse, don't rebuild)

- Razorpay Route split settlement + gateway-agnostic `payment_gateway_config`
- Category-wise commission (2–15%) model
- One category engine for **products AND services** (`category_attribute_schemas`)
- `translations` table + category images/badges/order (Burmese → **Tamil**)
- GST / TCS / TDS: `gstin`+`pan` sellers, `hsn_code`+`tax_rate` products, invoicing
- Seller KYC: GSTIN/PAN capture, penny-drop bank verification
- Buyer app + Seller app + Admin dashboard scaffolds
- App Store / Play Store checklists, metadata, build guides
- Push (FCM), AI, Cloudflare Stream, Google Maps, pincode addresses

---

## 4. Temple-specific work (what we actually build)

| Item | Notes |
|---|---|
| 7-column category tree → Tamil | Data, not code (see CATEGORY_TREE.md) |
| Service **scheduling / availability calendars** | Priests, astrologers, decorators — time-slot booking (verify cuckoo coverage) |
| **Build-a-Temple crowdfunding (FLAGSHIP)** | Directory of ~500 revered saints/gurus; GoFundMe-style campaigns; sponsor contributions; milestone proof + fund release; Bala-initiated + advertised |
| Membership + savings funds | Devotee/volunteer/temple/trust; renovation/festival/pilgrimage funds |
| Temple-admin role | Temple profile, events, volunteers, campaign progress |
| General donations | **Deferred** — temple/annadhanam/education/medical + 80G receipts in a later phase |
| God-image / festival banners | Auto-rotating home display (asset licensing TBD) |
| AI temple finder / planner | Claude API |
| Reskin + Tamil l10n | Branding, strings |

---

## 5. Pre-launch review gate — "done" = 5 journeys × 9 gates

### Actor journeys (walk happy path + failure path)
1. **Devotee/Buyer** — browse→search→detail→cart→pay→confirm→track→receive→rate; login/OTP, addresses, cancel/refund
2. **Product seller** (Flowers, Gift&Idol) — onboard/KYC→list→receive order→pack→deliver→payout
3. **Service provider** (Poojas, Events, Astro, Tourism, Sthapathi) — onboard→set services+availability→booking/quote→confirm→perform→payout
4. **Super Admin** — vendor approval, commission settings, refunds/disputes, payout monitoring, reports
5. **Temple Admin** — temple profile, events, donation tracking, volunteers

### Cross-cutting gates
1. Money correctness — paise as BIGINT, commission math, split amounts, rounding
2. Payment state machine — success/fail/pending/refund/partial
3. Cancellation & refund rules per type (product ≠ pooja ≠ hotel)
4. Auth & roles — RLS on every table
5. Notification map — which event → FCM → whom
6. Localization — English default + Tamil option on every screen incl. errors
7. Empty / error / edge states
8. Legal / store-readiness — GST invoice, privacy page, account-deletion page, refund policy
9. Non-functional — list/search perf, image sizes, load behavior

---

## 6. Build phases (fork-first)

- **Phase 0** Fork cuckoo → rename to TempleOTemple, strip non-temple features, reskin, Burmese→Tamil, confirm build runs
- **Phase 1** Category tree + product modules live (Flowers, Gift & Idol) end-to-end with Razorpay
- **Phase 2** Service-booking modules (Poojas, Events, Astro, Tourism, Sthapathi) + availability calendars
- **Phase 3** Donations + Membership + Savings funds + Temple-admin
- **Phase 4** AI assistant, God-image/festival display, loyalty
- **Phase 5** Pre-launch review gate (section 5) + store submission

---

## 7. Open items (sensible India defaults applied; confirm later)

- Vendor onboarding: admin-approved + KYC (inherited) — confirm self-serve toggle
- Refund/cancellation windows per category
- Delivery: Shiprocket vs vendor self-delivery for flowers (perishable!)
- God-image/festival banner asset licensing
- Donation 80G tax-receipt requirement
