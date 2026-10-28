# TempleOTemple — Feature Coverage Matrix (Build Master)

> **Purpose:** the definitive checklist of *every* feature from Bala's original plan,
> mapped to build phase, the proposal screen that shows it, and status.
> If it's not in this table, it's not in scope. This is the engineering source of truth.
>
> **Companion docs:** [MASTER_PLAN.md](MASTER_PLAN.md) · [CATEGORY_TREE.md](CATEGORY_TREE.md) · [DATA_MODEL.md](DATA_MODEL.md) · pitch = `Proposal.html`

**Legend — Status:** ✅ designed/visualized · 🟡 specced, not yet visualized · ⬜ to build
**Phases:** P0 Foundation · P1 Products · P2 Services · P3 Community · P4 Smart · P5 Launch

---

## 0. Global shell & navigation

| Feature | Phase | Proposal screen | Status |
|---|---|---|---|
| Top nav: Home, Marketplace, Services, Yatra, Membership, Donations, Wallet, Profile | P0 | Home | ✅ |
| Language: **English default**, Tamil toggle in settings (no launch picker) | P0 | Hero chips | ✅ |
| **Guest browsing** — no login to browse/search; OTP login only at purchase | P0 | Buyer journey | ✅ |
| Global search (all categories) | P1 | Home, Flowers | ✅ |
| Festival banner + auto-rotating deity image | P4 | Home | ✅ |
| Temple list / directory | P3 | — | 🟡 |
| Cart + Order status + Account | P1 | Checkout, Tracking | ✅ |
| Deliver-to / address book (pincode) | P1 | — | 🟡 |
| Summary cards: Today's Orders, Active Bookings, Donations, Membership, Vendor Revenue, Wallet Balance | P1–P3 | Admin dashboard | ✅/🟡 |

---

## 1. 🌸 Flowers Market

| Feature | Phase | Screen | Status |
|---|---|---|---|
| Fresh flowers: Jasmine (Malli), Rose, Marigold, Lotus, Mullai, Kanakambaram | P1 | Flowers | ✅ |
| Garlands: Temple / Wedding / VIP | P1 | Flowers | ✅ |
| Bulk orders: Temple / Wedding / Festival supply | P1 | Flowers | 🟡 |
| Archanai kit, festival/wedding/housewarming/ceremony kits | P1 | Flowers | ✅ |
| Vendor: product upload, inventory, pricing | P1 | Vendor onboarding | ✅ |
| Vendor: order management, delivery tracking, reviews | P1 | Seller accept, Tracking | ✅ |

## 2. 🎉 Event Management / Decorators

| Feature | Phase | Screen | Status |
|---|---|---|---|
| Temple events: Kumbabishekam, festival, mandapam, chariot decoration | P2 | Decorator | ✅ |
| Home events: housewarming, birthday, naming, anniversary | P2 | Decorator | 🟡 |
| Social: corporate, cultural, community | P2 | — | 🟡 |
| Performances: Nadhaswaram, Chendai Melam, Villupaatu, Karagattam, Peacock/Dummy-horse dance, Oyilattam, folk orchestra, spiritual music | P2 | Modules list | 🟡 |
| Decorator: portfolio upload | P2 | Decorator | ✅ |
| Decorator: event calendar, booking management | P2 | Decorator | 🟡 |
| Decorator: **quotation system** | P2 | Decorator (Request Quote) | ✅ |
| Decorator: customer ratings | P2 | — | 🟡 |

## 3. 🔱 Temple & Poojas (incl. Homam, Samskaras)

| Feature | Phase | Screen | Status |
|---|---|---|---|
| Daily / special / festival pooja | P2 | Pooja booking | ✅ |
| Navagraha, Lakshmi Kubera, Sathyanarayana pooja | P2 | — | 🟡 |
| Homam: Ganapathi, Sudarsana, Lakshmi, Ayush | P2 | Pooja booking | ✅ |
| Abhishekam: Rudra; Vratham: Varalakshmi | P2 | — | 🟡 |
| Shashtiabdapoorthi, Sathabhishekam | P2 | — | 🟡 |
| Samskaras: naming, upanayanam, ear piercing, griha pravesham, seemantham, puberty function | P2 | — | 🟡 |
| Pitru Karma / Tithi | P2 | — | 🟡 |
| Priest booking: temple priest / home priest | P2 | Pooja booking | ✅ |
| Wedding: Muhurtham planning, priest arrangements, traditional ceremonies | P2 | — | 🟡 |
| Priest dashboard: **availability calendar**, pricing, online booking, reviews | P2 | Pooja booking | ✅/🟡 |

## 4. 🎁 Gift & Idol

| Feature | Phase | Screen | Status |
|---|---|---|---|
| Temple related: lamps, pooja kits, rudraksha, spiritual books | P1 | Gift & Idol | ✅ |
| House related: décor, traditional products, festival items | P1 | Gift & Idol | 🟡 |
| Idols: Vinayagar, Murugan, Krishna; brass artifacts; handicrafts | P1 | Gift & Idol | ✅ |
| Return gift sets | P1 | Gift & Idol | ✅ |
| Seller: product catalog, inventory control, order processing | P1 | Vendor onboarding | ✅ |

## 5. 🚩 Yatra / Temple Tourism

| Feature | Phase | Screen | Status |
|---|---|---|---|
| Accommodation: temple guest house, hotels, dormitories | P2 | Yatra package | ✅/🟡 |
| Transport: taxi, van, bus tickets | P2 | Yatra package | ✅/🟡 |
| Pilgrimage packages | P2 | Yatra package | ✅ |
| Food: Annadhanam booking, group catering, restaurant | P2 | Yatra package | ✅/🟡 |
| Marriage hall booking (venue) | P2 | — | 🟡 |
| Yatra dashboard: trip planner, package manager, hotel booking engine, vehicle tracking | P2 | — | 🟡 |

## 6. 🔮 Astrologers

| Feature | Phase | Screen | Status |
|---|---|---|---|
| Astrology consultation, special services | P2 | — | 🟡 |
| Horoscope reading, marriage matching | P2 | — | 🟡 |
| Business astrology, vastu, numerology, gemstone advice | P2 | — | 🟡 |
| Astrologer dashboard: availability, pricing, booking, reviews | P2 | (shared with priest) | 🟡 |

## 7. 🏛️ Temple Architect (Sthapathi)

| Feature | Phase | Screen | Status |
|---|---|---|---|
| Agama services & consultation | P2 | — | 🟡 |
| Temple construction guidance, renovation (Jeernodharana) | P2 | — | 🟡 |
| Kumbabishekam services, Prathishta (consecration) | P2 | — | 🟡 |
| Yaga & homam arrangements, temple ritual auditing | P2 | — | 🟡 |
| Temple management consulting, idol sculptor | P2 | — | 🟡 |

---

## 8. 💰 Donations — **DEFERRED**

> General temple donations moved to a later phase (P4+). Crowdfunding (§10) replaces
> it as the community money feature at launch.

| Feature | Phase | Screen | Status |
|---|---|---|---|
| Temple donation, Annadhanam, educational support, medical assistance | P4+ | — | 🟡 deferred |
| Cause selection + amount presets | P4+ | — | 🟡 deferred |
| 80G tax receipt | P4+ | — | 🟡 deferred |

## 9. 👥 Membership & Savings

| Feature | Phase | Screen | Status |
|---|---|---|---|
| Individual: devotee registration, volunteer registration | P3 | — | 🟡 |
| Institutional: temple registration, trust registration | P3 | — | 🟡 |
| Savings plans: renovation fund, festival fund, pilgrimage savings | P3 | Membership & Savings | ✅ |
| Investment tracking: contributions, monthly savings, goal achievement | P3 | Membership & Savings | ✅ |
| Membership tiers (Gold etc.) | P3 | Membership & Savings | ✅ |

## 10. 🛕 Build-a-Temple Crowdfunding — **FLAGSHIP**

> GoFundMe-style platform to fund building temples for ~500 revered South-Indian
> saints/gurus/incarnations (e.g. Shirdi Sai Baba, Ramana Maharshi). Bala launches &
> advertises campaigns; devotees sponsor collectively.

| Feature | Phase | Screen | Status |
|---|---|---|---|
| Spiritual-figures directory (~500 saints/gurus) | P3 | — | 🟡 |
| Campaign per temple: goal, raised, progress %, est. completion | P3 | Build a Temple | ✅ |
| Sponsor contribution (preset + custom amounts) | P3 | Build a Temple | ✅ |
| Sponsor recognition ("name on the wall") + updates | P3 | Build a Temple | ✅ |
| Milestone proof upload + document repository | P3 | Temple Admin | 🟡 |
| Admin: launch campaign, verify milestone, release funds | P3 | Admin dashboard | 🟡 |
| Committee dashboard, vendor management (construction) | P3 | Temple Admin | 🟡 |

## 11. 💳 Wallet & Loyalty

| Feature | Phase | Screen | Status |
|---|---|---|---|
| Digital wallet: balance, add money | P3 | Wallet | ✅ |
| Payment methods: UPI, credit card, net-banking | P1 | Checkout | ✅ |
| Loyalty: reward points, membership tiers, referral program | P4 | Wallet | ✅ |

## 12. 🤖 AI Assistant

| Feature | Phase | Screen | Status |
|---|---|---|---|
| Temple finder | P4 | — | 🟡 |
| Service recommendation | P4 | AI Assistant | ✅ |
| Event planner | P4 | AI Assistant | ✅ |
| Travel guide | P4 | — | 🟡 |

---

## 13. 🛡️ Admin dashboards

| Feature | Phase | Screen | Status |
|---|---|---|---|
| **Super Admin:** vendor approval, user management | P1 | Admin dashboard | ✅ |
| **Super Admin:** revenue analytics, commission settings, reports | P1 | Admin dashboard | ✅ |
| **Super Admin:** refunds / disputes, payout monitoring | P1 | Admin dashboard | ✅ |
| **Temple Admin:** temple profile, event management | P3 | Temple Admin | ✅ |
| **Temple Admin:** donation tracking, volunteer coordination | P3 | Temple Admin | ✅ |
| **Vendor Admin:** product management, order management, customer service | P1 | Vendor onboarding, Seller accept | ✅/🟡 |

---

## 14. Cross-cutting (not a module, but must be built)

| Feature | Phase | Status |
|---|---|---|
| Guest browsing + phone OTP auth **only at purchase** + role routing | P0 | ⬜ |
| Razorpay checkout + Route split payout + webhook | P1 | ⬜ |
| Commission engine (category-wise %) | P1 | ⬜ |
| GST: GSTIN/PAN, HSN, tax invoices, TCS/TDS | P1 | ⬜ |
| Seller KYC + penny-drop bank verification | P1 | ⬜ |
| Cancellation / refund rules per category | P2 | ⬜ |
| Push notifications (FCM) event map | P1 | ⬜ |
| RLS on every table | P0+ | ⬜ |
| English-default + Tamil l10n on all screens | all | ⬜ |
| Shipping (Shiprocket) + COD serviceability | P1 | ⬜ |
| Store-readiness: privacy, account-deletion, refund policy pages | P5 | ⬜ |

---

## Coverage summary

- **Top-level modules:** 100% in scope and mapped.
- **Visualized in proposal:** all 7 verticals + donations, membership/savings, wallet,
  loyalty, AI, vendor onboarding, temple admin, super-admin.
- **🟡 Specced but not yet visualized:** granular sub-services (astrology sub-types,
  sthapathi services, performances, new-temple fund screens) — build in P2/P3.
- **⬜ Cross-cutting plumbing:** inherited from the cuckoo fork; wire up per phase.
