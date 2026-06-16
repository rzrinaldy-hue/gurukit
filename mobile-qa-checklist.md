# Mobile QA Checklist — GuruKit AI LP
Pre-ship test. Run BEFORE publishing.

## Device coverage (minimum)

| Device | Width | Status |
|--------|-------|--------|
| iPhone SE (3rd gen) | 375px | [ ] |
| Samsung Galaxy A series | 360px | [ ] |
| iPhone 14 | 390px | [ ] |
| Generic 320px (narrowest) | 320px | [ ] |
| iPad / Galaxy Tab | 768px | [ ] |
| Desktop laptop | 1280px | [ ] |

---

## 12 pre-ship checks

### 1. CTA above fold — 320px
- [ ] Open Chrome DevTools → Device toolbar → 320×568
- [ ] Hero headline + CTA button visible WITHOUT scroll
- [ ] Hero block height ≤ 560px on 320×568
- If fail: reduce hero padding OR trim hero copy

### 2. Tap targets ≥ 44×44px
- [ ] Primary CTA button (hero) — min-height 52px ✓ (set in CSS)
- [ ] Sticky CTA button — min-height 44px ✓
- [ ] FAQ `<summary>` accordion — min 44px high ✓
- [ ] Testimonial dot buttons — 24×24px visible + 44×44px clickable area ✓
- [ ] Footer legal links — check touch target on mobile

### 3. Font size ≥ 16px body
- [ ] Body text = 16px ✓ (set in :root)
- [ ] No copy at 12px or below in main content (only captions/footnotes OK)
- [ ] Pricing special price uses clamp (readable at 320px)

### 4. Line length 45–75 chars
- [ ] Body paragraphs max-width: 720px container ✓
- [ ] Hero headline ≤ 12 kata first line ✓ (verify actual render)
- [ ] No paragraph runs full 1280px on desktop

### 5. Page weight < 1 MB
- [ ] DevTools → Network → Disable cache → reload
- [ ] Total page weight < 1 MB (HTML + CSS + fonts + images)
- [ ] Hero image (hero.jpg) < 200 KB after CDN optimization
- [ ] Font: Plus Jakarta Sans — 2 weights loaded (400 + 700/800) ✓

### 6. Load < 3s on 3G sim
- [ ] Chrome DevTools → Network → Slow 3G
- [ ] Reload, TTI < 3 seconds
- [ ] FCP < 1.5 seconds
- [ ] Run: pagespeed.web.dev after deploy

### 7. No horizontal scroll
- [ ] Test at 320 / 375 / 414 / 768 / 1024 — no body overflow-x
- [ ] Check: hero proof-bar chips wrap correctly (flex-wrap: wrap ✓)
- [ ] Check: pricing special price doesn't overflow card
- [ ] Check: countdown timer fits on 320px

### 8. Tap-to-action links
- [ ] All `href="#pricing"` and `href="#form"` scroll correctly (scroll-behavior: smooth ✓)
- [ ] Footer legal links open correctly
- [ ] reCAPTCHA links open in new tab (rel="noopener noreferrer" ✓)

### 9. No `<form>` elements (HARD RULE — Scalev injects checkout)
- [ ] Confirm zero `<form>` tags in index.html ✓
- [ ] Confirm zero `<input>` tags ✓
- [ ] Confirm zero payment badge labels (BCA/QRIS/GoPay/ShopeePay) ✓
- [ ] Section 12 = CTA strip only ✓

### 10. Safe area insets (iPhone notch/home bar)
- [ ] Sticky CTA has `padding-bottom: calc(10px + env(safe-area-inset-bottom))` ✓
- [ ] Viewport meta has `viewport-fit=cover` ✓
- [ ] Test on actual iPhone SE / iPhone 14 if possible

### 11. OG / social meta
- [ ] og:title ≤ 60 chars ✓ (check: "GuruKit AI — RPP & Soal Selesai 15 Menit, Tanpa Lembur" = 55 chars)
- [ ] og:description ≤ 160 chars ✓
- [ ] og:image = lp-assets/hero.jpg (gets swapped to CDN URL post-process)
- [ ] og:locale = id_ID ✓
- [ ] twitter:card = summary_large_image ✓
- [ ] JSON-LD Product schema present ✓
- [ ] Test: developers.facebook.com/tools/debug (after deploy)
- [ ] Test: WhatsApp chat paste test

### 12. Accessibility baseline
- [ ] Single `<h1>` in hero ✓
- [ ] Heading hierarchy: H1 → H2 → H3 (no skips) ✓
- [ ] All `<img>` have descriptive `alt` text ✓
- [ ] FAQ uses native `<details>`/`<summary>` (keyboard accessible, screen-reader friendly) ✓
- [ ] CTA buttons have `:focus-visible` outline ✓
- [ ] `role="list"` on custom-styled lists ✓
- [ ] ARIA labels on carousel, countdown, live ticker ✓
- [ ] Color contrast: body text (#4A4845 on #FAFAF7) — check ≥ 4.5:1
  - #4A4845 on #FAFAF7 ≈ 6.8:1 ✓
- [ ] Primary teal (#1A7F6E on #FFFFFF) ≈ 5.2:1 ✓
- [ ] Run Lighthouse accessibility audit → target ≥ 90

---

## Browser check matrix

| Browser | Mobile | Desktop |
|---------|--------|---------|
| Chrome (latest) | [ ] ✅ Required | [ ] ✅ Required |
| Safari iOS (latest) | [ ] ✅ Required | — |
| Samsung Internet | [ ] ✅ Required | — |
| Firefox | [ ] Optional | [ ] Optional |

Samsung Internet = ~30% ID Android market share. Do not skip.

---

## Performance budget

| Metric | Target | Measured |
|--------|--------|---------|
| LCP | < 2.5s | [ ] |
| FID | < 100ms | [ ] |
| CLS | < 0.1 | [ ] |
| TTI | < 3.5s | [ ] |
| Mobile PageSpeed | ≥ 80 | [ ] |
| Total page weight | < 1 MB | [ ] |

---

## Launch blockers (do NOT publish if any fail)

- [ ] ❌ CTA below fold on 320px → BLOCK
- [ ] ❌ Page weight > 1.5 MB → BLOCK
- [ ] ❌ Mobile PageSpeed < 60 → BLOCK
- [ ] ❌ Horizontal scroll on any width → BLOCK
- [ ] ❌ `<form>` tag found in HTML → BLOCK (Scalev checkout broken)
- [ ] ❌ Payment method badges found in HTML → BLOCK
- [ ] ❌ OG image not rendering on FB Debugger → BLOCK

---

## Post-deploy monitoring

- [ ] Set up Microsoft Clarity (free heatmap) — catches mobile UX issues
- [ ] Set up Cloudflare Web Analytics (free RUM)
- [ ] Review heatmap after first 50 visitors — check drop-off section
- [ ] Re-scrape OG on FB Debugger 24h after deploy if social sharing active