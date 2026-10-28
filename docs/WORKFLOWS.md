# TempleOTemple — Business Workflows (Build Master)

> Every end-to-end business process — happy path, failure paths, and back-office.
> This is the operational source of truth; the proposal only shows the revenue-happy
> paths. Table names reference [DATA_MODEL.md](DATA_MODEL.md).
>
> Companion: [MASTER_PLAN.md](MASTER_PLAN.md) · [FEATURE_MATRIX.md](FEATURE_MATRIX.md) · [CATEGORY_TREE.md](CATEGORY_TREE.md)

**Order/booking states** and **payment states** are defined in DATA_MODEL.md; this doc
describes the transitions and who triggers them.

**Auth model:** browsing, searching, viewing details/reviews, and building a cart are
all **open to guests** — no login. **OTP login is required only at the purchase step**
(checkout / booking / sponsorship); the guest cart then moves to the new profile.
**Language:** English by default; Tamil is an in-app settings toggle (no launch picker).

---

## 1. Product order lifecycle (Flowers, Gift & Idol)
**Actors:** Buyer, Seller, Delivery, System, Admin

```
Buyer: add to cart → checkout → pay (Razorpay)
  ├─ payment captured → order=paid → notify seller(s)
  │     Seller: accept → pack → order=packed
  │       ├─ delivery assigned → order=shipped → tracking live
  │       │     └─ delivered → order=delivered → funds released to payout queue
  │       └─ seller rejects / out of stock → auto-refund → order=cancelled
  └─ payment failed/abandoned → order stays pending → auto-expire 30 min, release stock
```
**Failure paths**
- Delivery failed / buyer unavailable → reattempt; after N tries → return → refund (minus any policy fee).
- Partial stock → seller fulfils available, refunds rest.
- Perishable (flowers) → enforce same-day/slot cutoff; no late acceptance.

## 2. Service booking lifecycle (Poojas, Astrology)
**Actors:** Buyer, Service provider (priest/astrologer), System

```
Buyer: pick service → choose date/slot → pay (full or advance) → booking=confirmed
  Provider: gets alert → confirms slot (service_availability locked)
    ├─ performs on date → marks completed → payout released
    ├─ provider no-show → auto-reassign to backup provider OR full refund
    └─ buyer cancels → apply per-category cancellation rule → partial/full refund
```
**Rules**
- Slot is locked on payment to prevent double-booking.
- Reminder notifications T-24h and T-2h to both sides.

## 3. Quotation loop (Event Decorators, Sthapathi)
**Actors:** Buyer, Vendor, System

```
Buyer: submit requirement (date, venue, budget) → quotation=requested
  Vendor: reviews → sends quote (amount, scope) → quotation=sent
    ├─ Buyer accepts → pay (advance) → booking=confirmed → (service lifecycle §2)
    ├─ Buyer requests revision → vendor re-quotes (loop)
    └─ Buyer rejects / expires → quotation=rejected, closed
```

## 4. Payment → commission → payout settlement
**Actors:** System, Razorpay (Route), Vendor, Admin

```
Payment captured → split computed per order_item:
   vendor_amount = price − commission(category%) − gateway_share
Funds held until fulfillment confirmed (delivered/completed)
  → payout row created (status=pending)
  → settlement run (daily/weekly) → Razorpay Route transfer → payout=settled
```
**Rules**
- Minimum payout threshold (configurable, e.g. ₹500); below → roll to next cycle.
- Commission % per category from `platform_settings`.
- GST (18%) on commission; TCS/TDS ledger per vendor; monthly settlement report.
- RBI: buyer funds flow through the aggregator's escrow, never pooled in platform account.

## 5. Refund / cancellation / dispute
**Actors:** Buyer, Vendor, Admin, System

```
Refund trigger: buyer cancel | seller reject | delivery fail | dispute
  → check per-category window & fee
  → issue_refund(): reverse payment (Razorpay) + reverse/clawback payout + restock
  → order/booking=refunded; notify both parties
Dispute (buyer vs vendor):
  Buyer raises → status=disputed → Admin reviews evidence → decides refund / release
```
**Rules**
- Cancellation windows differ: product (before packed) ≠ pooja (T-24h) ≠ hotel (per policy).
- If payout already settled → clawback from next settlement.

## 6. Vendor onboarding / KYC / approval / lifecycle
**Actors:** Vendor, Admin, System

```
Vendor registers → submits GSTIN, PAN, bank, documents → kyc_status=pending
  Admin reviews → approve (penny-drop bank verify) → is_approved=true → can list
             └─ reject → vendor notified with reason → can resubmit
Ongoing: ratings tracked → low rating/violations → Admin warn → suspend → deactivate
```

## 7. Customer support / complaints
**Actors:** Buyer/Vendor, Support/Admin

```
User raises ticket (order-linked or general) → ticket=open
  → Support responds → in_progress
  → resolved → feedback → closed
Escalation: unresolved N days → Admin queue. Order disputes → link to §5.
```

## 8. Donation → receipt → temple disbursement — **DEFERRED (later phase)**
**Actors:** Donor, Temple, Admin, System

```
(Deferred to P4+. Crowdfunding §10 is the launch community-money feature.)
Donor: pick temple + cause + amount → pay → donation recorded
  → generate 80G tax receipt → periodic disbursement to temple bank
```

## 9. Savings fund → contribution → goal → release
**Actors:** Devotee, System, Temple/Admin

```
Devotee: create fund (renovation/festival/pilgrimage) + goal + monthly amount
  → auto/manual contributions (wallet/gateway) → saved_paise grows
  → goal reached → notify
     ├─ pilgrimage/festival → apply balance to a booking/order
     └─ temple fund → release to temple project (§10) or temple bank
```

## 10. 🛕 Build-a-Temple crowdfunding (FLAGSHIP)
**Actors:** Bala/Admin, Devotee/Sponsor, Temple committee, System

```
Admin (Bala): pick spiritual figure (from ~500 directory) → create campaign
   (goal, location, milestones) → status=live → ADVERTISE / promote to devotees
  Devotees: discover campaign → sponsor (preset/custom amount)
     → sponsor_campaign(): raised_paise + progress_pct up; name added to wall
  Goal reached → status=funded → construction begins
  Each milestone: committee uploads proof → Admin verifies
     → release_milestone_funds(): tranche released to temple → progress updates
  Completed → status=completed; sponsors notified; temple goes live in directory
Transparency: live goal/progress, milestone proofs, document repository, wall of sponsors
```
**Why flagship:** mission-driven reason to join the app; devotion → action; funnels
users into the surrounding marketplace. Bala seeds campaigns + advertising.

## 11. Promotions / festival campaigns
**Actors:** Admin, Vendors, Buyers

```
Admin creates campaign (festival banner, discount, featured category/vendors)
  → scheduled window → home banner + boosted ranking
  → vendors opt-in (promoted listing fee optional) → revenue lever
  → campaign ends → report (orders, GMV uplift)
```

## 12. Multi-vendor order splitting
**Actors:** Buyer, multiple Sellers, System

```
One cart may contain items from several vendors →
  on payment, split into per-vendor sub-orders (order_items grouped by vendor_id)
  each vendor fulfils independently; each has own payout
  buyer sees one order with per-vendor tracking
```

## 13. Delivery assignment
**Actors:** Seller, Delivery (self or Shiprocket), System

```
Order packed → delivery method:
  ├─ Vendor self-delivery → vendor marks shipped/delivered
  └─ Shiprocket → create shipment → courier assigned → AWB + tracking synced
COD: collected on delivery → remitted → reconciled against order
```

## 14. Vendor performance & rating
**Actors:** Buyers, System, Admin

```
After fulfillment → buyer rates → vendor rating recalculated
  Metrics: rating, on-time %, cancellation %, complaints
  Thresholds breached → Admin: warn → suspend listings → deactivate
  Good performers → eligible for featured placement (§11)
```

## 15. Admin commission & settings
```
Admin sets commission % per category, free-shipping threshold, min payout,
cancellation windows, gateway mode (test/prod) → stored in platform_settings
→ applied to all new orders/payouts immediately
```

## 16. Wallet top-up / spend / refund
```
Top-up: pay via gateway → wallet balance += amount
Spend: at checkout, wallet applied before gateway
Refund: credited to wallet (instant) or source (slower) per policy
Referral/loyalty bonus → credited as wallet_transactions
```

---

## Workflow → phase mapping
- **P1:** §1, §4, §5, §6, §12, §13, §15, §16
- **P2:** §2, §3, §14
- **P3:** §7, §8, §9, §10
- **P4:** §11
