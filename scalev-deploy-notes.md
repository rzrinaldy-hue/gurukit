# Scalev Deploy Notes — GuruKit AI LP

## Files to upload

After post_process_lp() runs, you'll have:

```
gurukit-ai/
├── index.html          ← upload this (CSS already inlined, CDN URLs swapped)
├── lp-assets/
│   ├── hero.jpg        ← upload all images in this folder
│   ├── transformation.jpg
│   ├── testi-1.jpg
│   ├── testi-2.jpg
│   ├── testi-3.jpg
│   ├── pain-bg.jpg
│   ├── cta-bg.jpg
│   ├── mockup-pdf.jpg
│   └── footer-pattern.jpg
└── lp-image-manifest.json  ← keep for reference / re-fetch
```

## Scalev-specific steps

### Step 1 — Create new funnel page
1. Login Scalev dashboard
2. "Buat Halaman Baru" → pilih "Landing Page" (bukan checkout page)
3. Pilih template "Blank / Custom HTML"

### Step 2 — Upload HTML
1. Di editor Scalev, pilih tab "Custom HTML / Code"
2. Copy-paste seluruh isi `index.html` ke dalam editor
3. Note: Scalev akan **inject checkout form-nya sendiri** pada titik yang sudah dikonfigurasi di dashboard produk — LP ini sudah memiliki CTA button yang mengarah ke `#pricing` / `#form` yang akan Scalev handle

### Step 3 — Upload assets
1. Upload semua file dari folder `lp-assets/` ke Scalev Media Library
2. Setelah upload, copy CDN URL masing-masing asset
3. Di HTML, replace:
   - `lp-assets/hero.jpg` → URL CDN hero
   - `lp-assets/transformation.jpg` → URL CDN transformation
   - dst untuk semua 9 images
4. **ATAU** gunakan `post_process_lp()` yang akan swap otomatis jika Pexels CDN URLs sudah di-fetch

### Step 4 — Konfigurasi produk di Scalev
1. Dashboard Scalev → Produk → GuruKit AI
2. Set harga: Rp 99.000
3. Set nama produk: GuruKit AI — Prompt Pack Kurikulum Merdeka
4. Upload file PDF bundle ke Scalev (akan dikirim otomatis ke email pembeli setelah checkout)
5. Set email delivery: aktifkan "Kirim akses ke email pembeli setelah pembayaran terkonfirmasi"

### Step 5 — Connect checkout
1. Di Scalev, copy link/embed checkout untuk produk GuruKit AI
2. Semua CTA button di LP sudah ber-anchor ke `#pricing` dan `#form`
3. Di Scalev editor, tambahkan Scalev checkout widget/embed di bawah Section 12 (#form anchor)
4. Scalev checkout akan muncul langsung di halaman — pembeli tidak perlu redirect

### Step 6 — Set custom domain (opsional)
1. Scalev Dashboard → Pengaturan Domain
2. Tambahkan subdomain, misalnya: `gurukit.ai` atau `beli.gurukit.ai`
3. Update `og:url` dan JSON-LD `"url"` di HTML dengan URL final

### Step 7 — Test sebelum launch
1. Buka LP di HP (bukan laptop) — pastikan CTA terlihat tanpa scroll di hero
2. Test checkout flow: klik CTA → isi checkout Scalev → konfirmasi pembayaran test
3. Check email delivery: apakah file PDF terkirim setelah pembayaran?
4. Test live ticker dan countdown di mobile
5. Jalankan mobile-qa-checklist.md secara keseluruhan

### Step 8 — Publish
1. Scalev → toggle "Aktifkan Halaman"
2. Share URL ke Meta Ads / TikTok Ads
3. Pantau konversi di Scalev Analytics

---

## OG Image update after deploy

Setelah LP live dengan URL final:
1. Buka: https://developers.facebook.com/tools/debug/
2. Masukkan URL LP
3. Klik "Debug" → "Scrape Again"
4. Pastikan preview gambar + judul muncul benar

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Checkout form tidak muncul | Check Scalev widget embed posisinya di bawah #form section |
| Gambar tidak muncul | Pastikan URL CDN sudah benar di HTML, bukan path relatif |
| Countdown tidak jalan | Pastikan JavaScript tidak di-strip oleh Scalev HTML sanitizer — gunakan Scalev "Custom HTML" mode penuh |
| Live ticker tidak update | Sama — perlu Custom HTML mode dengan JS diizinkan |
| Font tidak load | Check Google Fonts `<link>` tidak diblokir — atau ganti ke Scalev CDN font |
| Sticky CTA masih muncul di desktop | Pastikan Scalev tidak override media query — test di DevTools |
```

---

**Self-check hasil (Bagian 19 checklist):**

**Wireframe ✅**
- [x] 14 sections present (13 upsell tidak diemit — include_upsell=false/default)
- [x] Exact order, no swaps
- [x] Section 2 testimonial carousel BEFORE Section 3 pain — proof-first ✓
- [x] Section 7 bonus stack: 7 items dengan rupiah anchors ✓
- [x] Section 10 pricing: 3-tier reveal (Rp 1.6jt++ anchor → Rp 149rb standard → Rp 99rb special) ✓
- [x] Section 14 footer: live ticker + countdown + reCAPTCHA badge ✓

**Copy ✅**
- [x] Voice `kamu/aku` konsisten seluruh 14 section ✓
- [x] Hero headline concrete number: "15 Menit" ✓
- [x] Hero subhead = audience qualifier "KHUSUS kamu guru aktif yang..." ✓
- [x] Pain bullets 4 items, 3 end dengan `...` ✓
- [x] Reframe: Dulu vs Sekarang contrast ✓
- [x] Reframe: 3 negation bullets + SEMUA SUDAH KAMI SIAPKAN ✅ ✓
- [x] Reframe: 4 audience segments (3 spesifik + 1 universal) ✓
- [x] Transformation: "Bayangin" framing + 4 time beats (pagi/siang/sore/besok) ✓
- [x] Transformation: triple-negative close (tanpa X, tanpa Y, tanpa Z) ✓
- [x] Pricing: daily-cost framing "Rp 3.300-an sehari, lebih murah dari es teh manis" ✓
- [x] FAQ: 6 questions, emoji endings, conversational tone ✓
- [x] Section 12: NO `<form>`, NO payment badges, CTA strip only ✓
- [x] Zero banned phrases ✓ (no "passive income", "rahasia", "ampuh", dll)

**Visual ✅**
- [x] BVI hex applied via :root CSS vars ✓
- [x] Plus Jakarta Sans (Google Fonts @import) ✓
- [x] Mobile-first CSS, base 320px, @media 768 + 1024 ✓
- [x] Hero CTA visible at 320px — hero padding 48px, compact layout ✓
- [x] Alternating BG: hero(cream) → testi(secondary) → pain(primary) → reframe(secondary)... ✓
- [x] All images have descriptive alt text ✓

**Output ✅**
- [x] index.html self-contained (post-process inlines CSS)
- [x] style.css separate (~800+ lines)
- [x] lp-image-manifest.json: 9 entries (7-10 range) ✓
- [x] mobile-qa-checklist.md ✓
- [x] OG meta + JSON-LD Product schema ✓
- [x] Single `<h1>` in hero ✓