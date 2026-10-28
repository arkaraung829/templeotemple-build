# TempleOTemple — Category Tree (Build Master)

> The corrected 7-vertical menu → maps directly to the `categories` table
> (`parent_id` self-reference). Each top-level category = one `vendor_type`.
> Tamil names (`name_ta`) to be filled by Bala / a Tamil reviewer.
>
> **Fixes applied** from the original menu review:
> - `BOOKS` moved from Tourism → Gift & Idol
> - `Marriage Hall` kept under Tourism but tagged as **venue**
> - Poojas split into **Poojas/Homams** vs **Samskaras (life functions)**
> - Events split into **Decoration** vs **Performances**
> - Kumbabishekam disambiguated: ritual (Poojas) vs construction service (Sthapathi)
> - Typos fixed (Festival, package, Marriage, decoration, Ceremony, Spiritual, Orchestra, puberty)

---

## 1. 🌸 Flowers Market  → vendor_type: `flower_seller`  (product)
- Fresh Flowers: Jasmine (Malli), Rose, Marigold, Lotus, Mullai, Kanakambaram
- Garlands: Temple, Wedding, VIP
- Kits: Archanai Kit, Festival Package, Marriage Kit, Housewarming Kit, Ceremony Kit
- Bulk Supply: Temple, Wedding, Festival

## 2. 🎉 Event Management  → vendor_type: `decorator`  (service / quotation)
- **Decoration:** Temple Deity, Marriage, School Event, Mandapam, Kumbabishekam (décor), Chariot, Housewarming, Birthday, Naming, Anniversary, Corporate, Cultural, Community
- **Performances:** Nadhaswaram, Chendai Melam, Villupaatu, Karagattam, Peacock Dance, Dummy Horse Dance, Oyilattam, Traditional Folk Orchestra, Spiritual Music events

## 3. 🔱 Temple & Poojas  → vendor_type: `priest`  (service / booking)
- **Poojas:** Daily, Special, Festival, Navagraha, Lakshmi Kubera, Sathyanarayana
- **Homam / Havan:** Ganapathi, Sudarsana, Lakshmi, Ayush
- **Abhishekam / Vratham:** Rudra Abhishekam, Varalakshmi Vratham
- **Milestone poojas:** Shashtiabdapoorthi, Sathabhishekam
- **Samskaras (life functions):** Naming, Upanayanam, Ear Piercing, Griha Pravesham, Seemantham, Puberty Function, Wedding Function, Bhoomi Pooja, Pitru Karma / Tithi
- **Priest services:** Temple Priest, Home Priest, Muhurtham Planning, Priest Arrangements

## 4. 🎁 Gift & Idol  → vendor_type: `gift_seller`  (product)
- Temple: Lamps, Pooja Kits, Rudraksha, Spiritual Books, **Books**
- House: Home Décor, Traditional Products, Festival Items
- Idols & Craft: Vinayagar, Murugan, Krishna, Brass Artifacts, Handicrafts
- Gifts: Religious Gifts, Return Gifts, Decorative Items, Spiritual Gift Sets

## 5. 🚩 Temple Tourism  → vendor_type: `tourism_vendor`  (service / booking)
- Accommodation: Temple Guest House, Hotels, Dormitories
- Transport: Taxi, Van, Bus Tickets
- Packages: Pilgrimage Packages
- Food: Annadhanam, Group Catering, Restaurant
- Venues (tag): Marriage Hall

## 6. 🔮 Astrologers  → vendor_type: `astrologer`  (service / booking)
- Astrology Consultation, Special Astrology Services
- Horoscope Reading, Marriage Matching
- Business Astrology, Vastu Consultation, Numerology, Gemstone Advice

## 7. 🏛️ Temple Architect (Sthapathi)  → vendor_type: `sthapathi`  (service / quotation)
- Agama Services, Agama Consultation
- Temple Construction Guidance, Temple Renovation (Jeernodharana)
- Kumbabishekam Services, Prathishta (Consecration) Services
- Yaga & Homam Arrangements, Temple Ritual Auditing
- Temple Management Consulting, Idol Sculptor

---

## Platform categories (not vendor verticals)
- 🛕 **Build-a-Temple Crowdfunding (flagship)** · 👥 Membership & Savings · 💳 Wallet & Loyalty · 🤖 AI Assistant
- 💰 General Donations — *deferred to a later phase*

## `categories` table mapping
```
categories(
  id, parent_id (self FK), vendor_type,
  name_en, name_ta, slug, icon, image_url,
  fulfillment_type ENUM('product','booking','quotation'),
  display_order, region_priority, is_active
)
```
- Top-level row → `parent_id = NULL`, carries `vendor_type` + `fulfillment_type`.
- Children inherit vendor_type; leaf categories hold listings.
