# Pro House Official Website — Project Brief / Handoff

## Context

Pro House (برو هاوس) is a healthy-food restaurant brand in Jeddah, Saudi Arabia, running two
branches and a meal-subscription service. The owner, Ziyad, asked for an official public
website, primarily to fill the "Website" field on the brand's Google Business Profile and to
act as a canonical link for the apps and social profiles.

The brief originally specified Google Sites at `prohouse-sa`. That constraint was lifted —
Ziyad confirmed the platform does not matter. Verified independently: Google Business Profile
accepts any website URL, provided it represents the actual business, returns HTTP 200, and is
crawlable without login. Google Sites was never required.

## What was built

A two-page static site — no framework, no build step, no dependencies.

```
prohouse-site/
├── index.html          Arabic (RTL) — primary
├── en/index.html       English (LTR)
├── assets/
│   ├── style.css       design tokens + all styling
│   ├── main.js         mobile nav, header state, scroll reveal (decoration only)
│   └── brand/          logo variants + photography
└── HANDOFF-PROMPT.md   this file
```

Sections on both pages: hero, our story, what we offer, subscriptions, branches + delivery
area, app download, contact, privacy.

## Design system

Light-first, per the identity deck: **white canvas, yellow blocks, black type.**
Yellow `#F7DC4E` is a surface colour, never a text colour — yellow on white is 1.4:1 and
fails WCAG badly, while black on yellow is ~15:1. Do not invert this into a dark theme.

- Yellow `#F7DC4E` · Black `#000000` · Red `#ff5151` · Gray `#575454` · Purple `#5E17EB`
- Motifs taken from the deck: organic yellow blob, halftone dot pattern
- Display face: Anton (Latin only). Arabic display and all body text: IBM Plex Sans Arabic
- Spacing on an 8px scale, all tokens in `:root`

**Font note:** the identity deck specifies Impact + Frutiger. Neither is usable on the web —
Frutiger needs a paid webfont licence, and Impact is absent from Android. Anton and IBM Plex
Sans Arabic are the licence-clean substitutes. The deck itself does not use Impact/Frutiger in
its own layouts either, so the written spec and the applied spec already diverge.

## Accessibility, SEO, performance

- Semantic landmarks, skip link, visible focus ring, labelled controls, keyboard-operable nav
- Full `prefers-reduced-motion` support — all motion disabled, content still visible
- Content lives in the HTML; JavaScript adds nothing and the page works without it
- `hreflang` pair + `x-default`, canonical, Open Graph, Twitter card
- JSON-LD: two `Restaurant` entities (one per branch) plus a `WebSite` entity
- Photography re-encoded PNG → JPEG: 2417 KB → 282 KB (−88%)

## Confirmed facts (do not alter without checking)

- Branches: **Al Rawdah** — 4234 Al Kayyal St, Al Rawdah District, Jeddah ·
  **Al Shati** — Corniche Road, Jeddah Square Building, Jeddah
- Delivery: Jeddah, Makkah, Taif (two branches, three delivery cities)
- Phone / WhatsApp: `0540024717` / `+966540024717`
- Email: cs.prohouse@gmail.com
- Web store: https://app.techrar.com/prohouse
- App Store: id6466739432 · Google Play: `com.yumealz.prohouse`
- Instagram / TikTok: `@prohouse.sa` · Snapchat: https://snapchat.com/t/CPG6evAH

## Conversion decision

The primary call to action is **Subscribe / اشترك الآن → the Techrar web store**, not an app
install. The store is a complete web checkout, so sending people there removes the
store-page → download → sign-up funnel entirely. App-store buttons are deliberately secondary.

Prices are **not** duplicated onto this site: they change, a stale copy would mislead, and the
live store already shows them.

## Before publishing

1. **Replace `__SITE_URL__`** in `index.html` and `en/index.html` with the real domain.
   `grep -rn __SITE_URL__ .` finds every occurrence.
2. **Add opening hours.** Both branch cards currently say "message us on WhatsApp to confirm".
   Search for the `<!-- Opening hours -->` comments. Add `openingHoursSpecification` to the
   JSON-LD at the same time.
3. **Replace the photography.** See the known gaps below.

## Known gaps

- **Photography is weak.** The only images available came out of the identity PDF at 450×450
  and 756×945, and the two larger ones have a logo and marketing text baked into the pixels.
  They are being used as hero and story images because nothing better exists. Real photos —
  dishes, both branch interiors, exteriors — would raise the quality of this site more than
  any other single change. `@prohouse.sa` on Instagram is the likely quick source.
- **No purpose-built OG image.** Social shares currently fall back to a food photo at the wrong
  aspect ratio. WhatsApp is the main sharing channel in Saudi, so a proper 1200×630 image is
  worth commissioning.
- **Logo is raster only.** No SVG exists. Fine for web; request vector before any print use.
- **No analytics.** Deliberate. If added, prefer a cookieless tool (Cloudflare Web Analytics,
  Plausible) — Google Analytics would make the privacy statement inaccurate as written.
- **No contact form.** Also deliberate: `tel:`, `wa.me` and `mailto:` links convert better and
  keep the site free of personal-data collection, avoiding Saudi PDPL obligations.

## App privacy policy — not this site's job

The app ships under the bundle id `com.yumealz.prohouse`, i.e. it is built on the Yumealz /
Techrar platform (a Jeddah meal-subscription SaaS). Both app stores mandate a privacy policy,
so one already exists in the listings — almost certainly the vendor's, at
https://www.yumealz.com/privacy-policy/

The privacy section on this website covers **the website only**, which genuinely collects
nothing. Do not submit this URL as the app's privacy policy in either store. Worth confirming
with Techrar which URL is attached to the listings.

## Constraint from the owner

Do not modify Google Cloud projects, API configuration, or Google Maps / Business Profile
accounts. Scope is the website only.

## Open decisions

- [ ] Domain — a real domain is strongly recommended over a `*.github.io` URL, which
      undercuts the word "official" in the one place it matters most (the Maps listing)
- [ ] Hosting account owner — hosting a client's official site on a personal account means the
      site dies with that account. Prefer an account owned by the business.
- [ ] Confirm approval to publish publicly
