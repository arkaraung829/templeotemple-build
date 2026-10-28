# TempleOTemple — Data Model (Build Master)

> High-level Supabase (PostgreSQL) schema. Money is stored as **paise (BIGINT)**.
> Every table has **RLS**. Inherited/adapted from the cuckoo fork where noted.
>
> Companion: [FEATURE_MATRIX.md](FEATURE_MATRIX.md) · [CATEGORY_TREE.md](CATEGORY_TREE.md) · [MASTER_PLAN.md](MASTER_PLAN.md)

---

## Identity & roles
- **profiles** — id (→ auth.users), name, phone, email, role, locale, created_at
  - role ENUM: `buyer`, `seller`, `priest`, `astrologer`, `decorator`, `sthapathi`, `tourism_vendor`, `temple_admin`, `super_admin`
- **addresses** — id, profile_id, line, city, district, state, **pincode**, is_default
- **vendor_profiles** — id, profile_id, vendor_type, shop_name, **gstin**, **pan**, bank_account, kyc_status, rating, is_approved
- **temples** — id, name, city, deity, profile_id (temple_admin), description, image_url

## Catalog
- **categories** — id, parent_id, vendor_type, name_en, name_ta, slug, icon, image_url, **fulfillment_type** (`product`/`booking`/`quotation`), display_order, region_priority, is_active
- **listings** — id, vendor_id, category_id, title_en, title_ta, description, **price_paise**, **hsn_code**, **tax_rate**, stock, is_active
- **listing_images** — id, listing_id, url, display_order
- **listing_attributes** — id, listing_id, key, value (flexible per-category attrs)
- **reviews** — id, listing_id, buyer_id, rating, comment, created_at

## Services & scheduling
- **service_availability** — id, vendor_id, date, slot_start, slot_end, is_booked
- **bookings** — id, buyer_id, vendor_id, listing_id, scheduled_at, status, **amount_paise**, address_id
  - status ENUM: `requested`, `quoted`, `confirmed`, `in_progress`, `completed`, `cancelled`
- **quotations** — id, booking_id, vendor_id, **amount_paise**, notes, status (`sent`/`accepted`/`rejected`)

## Orders & payments
- **orders** — id, buyer_id, status, **subtotal_paise**, **delivery_paise**, **total_paise**, address_id, created_at
  - status ENUM: `pending`, `paid`, `packed`, `shipped`, `delivered`, `cancelled`, `refunded`
- **order_items** — id, order_id, listing_id, vendor_id, qty, **unit_price_paise**, **commission_paise**
- **payments** — id, order_id|booking_id, gateway (`razorpay`), gateway_payment_id, **amount_paise**, status (`created`/`captured`/`failed`/`refunded`), method (`upi`/`card`/`netbanking`/`cod`)
- **payouts** — id, vendor_id, order_item_id, **amount_paise**, **commission_paise**, route_transfer_id, status (`pending`/`settled`), settled_at
- **payment_gateway_config** — id, gateway, mode (`test`/`prod`), keys, is_active *(gateway-agnostic)*
- **platform_settings** — key, value (commission % per category, free-shipping threshold, etc.)
- **refunds** — id, payment_id, **amount_paise**, reason, status, created_at

## Build-a-Temple crowdfunding (FLAGSHIP)
- **spiritual_figures** — id, name_en, name_ta, description, image_url, region, is_active *(directory of ~500 revered saints/gurus/incarnations)*
- **temple_campaigns** — id, spiritual_figure_id, temple_id (nullable until built), title, location, **goal_paise**, **raised_paise**, progress_pct, est_completion, status (`draft`/`live`/`funded`/`building`/`completed`), launched_by (admin), is_featured
- **sponsorships** — id, temple_campaign_id, sponsor_id, **amount_paise**, display_name (wall of names), created_at
- **campaign_milestones** — id, temple_campaign_id, title, proof_document_id, **released_paise**, verified_by, verified_at

## Membership & funds
- **memberships** — id, profile_id, type (`devotee`/`volunteer`/`temple`/`trust`), tier, started_at
- **savings_funds** — id, profile_id, fund_type (`renovation`/`festival`/`pilgrimage`), **goal_paise**, **saved_paise**, monthly_paise, goal_date
- **fund_contributions** — id, savings_fund_id, **amount_paise**, created_at

## Donations — DEFERRED (later phase)
- **donations** — id, donor_id, temple_id, cause, **amount_paise**, receipt_80g_no, created_at

## Wallet & loyalty
- **wallets** — id, profile_id, **balance_paise**
- **wallet_transactions** — id, wallet_id, type (`topup`/`spend`/`refund`/`referral`), **amount_paise**, ref, created_at
- **loyalty** — id, profile_id, points, tier, referral_code

## System
- **notifications** — id, profile_id, type, payload, fcm_sent, read_at
- **documents** — id, temple_id|vendor_id, name, url, type *(committee doc repository)*

---

## Key atomic functions (Postgres)
- `process_order_payment()` — mark paid, split into payouts (commission retained), decrement stock
- `confirm_booking()` — lock availability slot, create payout row
- `issue_refund()` — reverse payment + payout, restock
- `contribute_to_fund()` — wallet/gateway debit → savings_funds + fund_contributions
- `sponsor_campaign()` — pay → sponsorships + bump temple_campaigns.raised_paise/progress_pct
- `release_milestone_funds()` — admin verify → release tranche to temple, update progress

> **Note:** English-default + login-at-purchase are app-flow decisions, not schema
> changes. `profiles.locale` already stores language; guest carts move to the
> profile on OTP login at checkout.

## RLS principles
- Buyers see only their own orders/bookings/wallet.
- Vendors see only their own listings/orders/payouts.
- Temple admins see only their temple's donations/events/volunteers.
- Super admin bypass via service role (server-side only).
