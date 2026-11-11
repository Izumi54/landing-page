# Linktree Landing Page — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Membuat halaman link-bio statis satu halaman yang menampilkan link GitHub, referral Dicoding, dan portfolio, responsif di mobile/iPad/desktop, lalu publish ke GitHub Pages dengan subdomain `links.izumi-dev.my.id`.

**Architecture:** Satu halaman statis tanpa build step: `index.html` (struktur + konten) dan `style.css` (desain mobile-first). Tidak ada JavaScript kecuali diperlukan nanti. Deploy via GitHub Pages dengan file `CNAME` untuk custom domain.

**Tech Stack:** HTML5, CSS3 (vanilla), GitHub Pages.

**Spec:** Brainstorming session (halaman link-bio simpel ala Linktree; tema gelap bersih; HTML+CSS polos; hosting gratis; subdomain `links.izumi-dev.my.id`).

## Global Constraints

- Tanpa build tools, tanpa npm, tanpa framework — hanya `index.html` + `style.css` (+ `CNAME` saat deploy).
- Mobile-first; harus teruji di lebar 320px, 768px, 1024px, 1440px.
- Kontras teks ≥ 4.5:1 (WCAG 2.1 AA); semua link bisa diakses dengan keyboard (Tab + Enter).
- Link terbuka di tab baru (`target="_blank"` + `rel="noopener noreferrer"`).
- Label link (urutan dan teks): `GitHub`, `Dicoding Referral`, `Portfolio`.
- URL link persis: `https://github.com/izumi54`, `https://dicoding.id/p27-dias1`, `https://izumi-dev.my.id`.
- Desain: tema gelap, satu warna aksen (bukan gradien ungu/indigo default AI), spacing pakai skala 4px, tanpa shadow berlebih.
- Tanpa lorem ipsum — konten teks final: nama tampilan `Izumi` (dapat diubah mudah).

## Review Focus

- **Layout di 320px:** tombol link tidak boleh terpotong atau overflow horizontal — verifikasi Task 3 di lebar 320px.
- **Aksesibilitas keyboard:** urutan Tab masuk akal dan focus ring terlihat — verifikasi Task 3.
- **Kontras:** teks sekunder (footer) tetap ≥ 4.5:1 di tema gelap — verifikasi Task 3.
- **Custom domain:** file `CNAME` berisi `links.izumi-dev.my.id` tanpa `https://` — verifikasi Task 4.
- **Link keluar:** ketiga URL harus benar dan `rel="noopener"` ada — verifikasi Task 2.

---

### Task 1: Scaffold struktur halaman

**Files:**
- Create: `index.html`

**Interfaces:**
- Produces: elemen `<header>` (avatar + nama `Izumi`), `<main>` dengan `<ul>` berisi 3 `<a>`, `<footer>` — struktur yang dipakai Task 3.

- [ ] **Step 1: Buat `index.html`**

Struktur persis:

```html
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Izumi — Links</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="page">
    <header class="profile">
      <div class="avatar" aria-hidden="true">I</div>
      <h1>Izumi</h1>
      <p class="tagline">Developer &amp; learner</p>
    </header>
    <main>
      <ul class="links">
        <li><a href="https://github.com/izumi54" target="_blank" rel="noopener noreferrer">GitHub</a></li>
        <li><a href="https://dicoding.id/p27-dias1" target="_blank" rel="noopener noreferrer">Dicoding Referral</a></li>
        <li><a href="https://izumi-dev.my.id" target="_blank" rel="noopener noreferrer">Portfolio</a></li>
      </ul>
    </main>
    <footer>
      <small>© 2026 Izumi</small>
    </footer>
  </div>
</body>
</html>
```

- [ ] **Step 2: Verifikasi struktur**

Run: `python3 -m http.server 8000` lalu buka `http://localhost:8000`
Expected: halaman terbuka, 3 link tampil (tanpa styling), klik membuka URL benar di tab baru.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: scaffold linktree page structure"
```

---

### Task 2: Styling tema gelap & layout link-bio

**Files:**
- Create: `style.css`

**Interfaces:**
- Consumes: struktur HTML Task 1 (`.page`, `.profile`, `.avatar`, `.links`, `footer`).
- Produces: stylesheet lengkap yang dipakai Task 3 untuk override breakpoint.

- [ ] **Step 1: Buat `style.css` dengan token & dasar**

Mobile-first. Keputusan desain (sudah disepakati):

- Warna: background `#0f1115`, surface kartu `#1a1d24`, teks utama `#e8eaed` (kontras ~14:1), teks sekunder `#9aa0a6` (kontras ~5.6:1), aksen `#4cc38a` (hijau teal — bukan ungu default).
- Spacing scale: 4/8/12/16/24/32px.
- `.page`: `max-width: 500px; margin: 0 auto; padding: 32px 16px;` — di mobile lebar penuh, di desktop kolom tengah.
- `.avatar`: 80px bulat, background aksen, huruf `I` tebal.
- `.links a`: block, padding 14px 16px, background surface, border `1px solid #2a2e37`, border-radius 8px (bukan 2xl), teks center, warna teks utama; hover/focus: border aksen + background sedikit terang; focus ring terlihat (`outline: 2px solid` aksen, offset 2px).
- Transisi halus `150ms` hanya pada border/background.
- `footer small`: teks sekunder, center.

- [ ] **Step 2: Verifikasi visual di server lokal**

Run: `python3 -m http.server 8000`
Expected: tampilan gelap rapi, kartu link sejajar, tidak ada gradien ungu/indigo.

- [ ] **Step 3: Commit**

```bash
git add style.css
git commit -m "feat: add dark theme linktree styles"
```

---

### Task 3: Responsif (320/768/1024/1440) + aksesibilitas

**Files:**
- Modify: `style.css`

**Interfaces:**
- Consumes: base style Task 2.

- [ ] **Step 1: Tambahkan aturan responsif**

- Base (≤767px): sudah mobile-first dari Task 2.
- `@media (min-width: 768px)`: `.page` padding naik ke `48px 24px`; avatar 96px; link padding sedikit lebih besar.
- `@media (min-width: 1024px)`: `max-width: 480px` tetap (pola link-bio), spacing vertikal antar link `gap: 12px`.
- Tidak ada perubahan layout drastis — halaman selalu satu kolom.

- [ ] **Step 2: Verifikasi keyboard & kontras**

- Tab melalui halaman: fokus bergerak avatar→link1→link2→link3, focus ring terlihat.
- Cek kontras dengan DevTools accessibility pane: semua teks ≥ 4.5:1.
- Uji lebar 320, 768, 1024, 1440px (DevTools device toolbar): tidak ada overflow horizontal.

- [ ] **Step 3: Commit**

```bash
git add style.css
git commit -m "feat: responsive breakpoints and a11y focus styles"
```

---

### Task 4: Deploy GitHub Pages + custom domain

**Files:**
- Create: `CNAME` (isi: `links.izumi-dev.my.id`)

**Interfaces:**
- Produces: situs live di `https://links.izumi-dev.my.id`.

- [ ] **Step 1: Push repo ke GitHub**

Repo `landing-page` dengan branch `main`.

- [ ] **Step 2: Buat file `CNAME`**

Isi satu baris tanpa `https://`: `links.izumi-dev.my.id`

- [ ] **Step 3: Aktifkan GitHub Pages**

Settings → Pages → Source: `main` / root. Tunggu deploy selesai.

- [ ] **Step 4: Arahkan DNS subdomain**

Di DNS provider domain `izumi-dev.my.id`, tambahkan record CNAME `links` → `<username>.github.io`.

- [ ] **Step 5: Verifikasi live**

Buka `https://links.izumi-dev.my.id` dari HP dan desktop.
Expected: halaman tampil, HTTPS aktif, 3 link berfungsi.

- [ ] **Step 6: Commit & push**

```bash
git add CNAME
git commit -m "chore: add custom domain CNAME"
git push -u origin main
```
