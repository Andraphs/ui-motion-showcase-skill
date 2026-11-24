# Motion Bible — Anti-AI Slop, Fisika Gerak & Resep Visual

## 1. Daftar Larangan Keras (Anti-AI Slop Rules)
1. **DILARANG `opacity: 0 → 1` polos** untuk memunculkan elemen utama atau teks. Gunakan *clip-path/overflow masking* berarah + pergeseran halus (`translateY: 20px → 0` atau `scale: 0.96 → 1`).
2. **DILARANG `ease-in-out` standar atau `linear`** untuk transisi UI (kecuali *scroll/marquee* atau *film grain*).
3. **DILARANG animasi memantul berlebihan (*bouncy/cartoony*)** dengan *damping* rendah. UI SaaS harus terasa presisi, berat, dan mahal.
4. **DILARANG efek *neon glow* berlebihan** atau gradien warna-warni generik yang bukan bagian dari *design tokens* produk.
5. **DILARANG menampilkan 1 halaman dashboard penuh secara statis dari jauh.** Penonton tidak sempat membaca. Wajib *zoom/crop* dan isolasi 1 komponen pahlawan per adegan.
6. **DILARANG memunculkan seluruh elemen anak bersamaan.** Wajib *stagger* sebesar **2–4 frame** per elemen.

---

## 2. Fisika Gerak & Easing Terkunci (60 fps)

### Kurva Utama (Heavy Deceleration — Cepat Masuk, Mendarat Sutra)
- **CSS/Bezier Equivalent:** `cubic-bezier(0.16, 1, 0.3, 1)` (Expo Out)
- **Remotion `Easing`:** `Easing.bezier(0.16, 1, 0.3, 1)`
- **Remotion `spring()` Presets:**
  - `SNAPPY_UI` (kartu, modal, teks): `{ damping: 26, stiffness: 220, mass: 0.85 }` (hampir nol overshoot).
  - `TACTILE_PRESS` (klik tombol, toggle): `{ damping: 20, stiffness: 380, mass: 0.5 }`.
  - `CAMERA_GLIDE` (pan/zoom kamera): `{ damping: 32, stiffness: 95, mass: 1.2 }`.

### Ritme & Pacing (Contrast Principle)
- **Aksi cepat (14–22 frame / 0.23–0.36 detik)** → diikuti **Hold/Breathing Room (48–72 frame / 0.8–1.2 detik)** agar mata penonton sempat mencerna angka/pesan sebelum kamera bergerak lagi.

---

## 3. Kamera Sinematik & Flowing Spline (DNA `labs-studio`)

Perlakukan UI seperti objek fisik di dalam studio, bukan rekaman layar datar:
- **Subtle 3D Perspective:** Bungkus stage dengan `perspective: 1400px`. Terapkan kemiringan halus (`rotateX: 4deg–10deg`, `rotateY: -8deg–8deg`) saat transisi antar fitur, lalu ratakan kembali (`0deg`) saat penonton fokus membaca data.
- **Flowing Spline Camera:** Ketika kamera melewati 3 titik (misal A → B → C), jangan biarkan kecepatan menyentuh `0` secara kaku di titik B. Gunakan interpolasi spline mulus atau *overlapping spring* agar kamera terus mengalir (*continuous drift* sebesar 1–2% scale bahkan saat hold).
- **Depth Parallax:** Pisahkan stage menjadi 3 kedalaman:
  1. `Background Layer` (grid editorial / matte / liquid gradient): bergerak **0.25x** dari pergerakan kamera.
  2. `Hero UI Layer` (dashboard / kartu utama): bergerak **1.0x**.
  3. `Floating Callouts / Cursor / Toast`: bergerak **1.15x–1.25x** dengan bayangan lebih lembut dan jauh.
- **True Optical Motion Blur:** Bungkus adegan transisi cepat dan *kinetic typography* dengan `<CameraMotionBlur shutterAngle={180} samples={8}>` (dari `@remotion/motion-blur`).

---

## 4. Kursor Manusiawi (Human Cursor Protocol)

Kursor AI yang bergerak lurus kaku langsung merusak ilusi video studio. Wajib terapkan siklus 4 fase ini:
1. **Curved Approach:** Gerakkan kursor dari titik A ke target tombol B menggunakan kurva **Quadratic Bezier** (beri offset lengkungan 40–90px, bukan garis lurus), dengan kurva kecepatan *Expo Out*.
2. **Hover & Anticipation Pause:** Saat kursor menyentuh area tombol, langsung aktifkan *hover state* (border lebih terang / background shift) lalu **JEDA 4–6 frame** (seolah mata manusia sedang memastikan tombolnya).
3. **Tactile Click:** Kompres tombol & kursor (`scale: 0.94` selama **3 frame**), lalu lepas kembali ke `1.0`. Kunci frame tepat saat klik ke `SFX_CUES` (`click`).
4. **Consequence (Chain Reaction):** 2–4 frame setelah klik, picu perubahan UI (modal muncul, toggle bergeser, atau angka terkalkulasi ulang).

---

## 5. Resep Visual & Finishing Studio

- **Studio Texture:** Selalu tambahkan overlay **Film Grain halus (opacity 2–3%)** yang berganti *seed* setiap 2 frame agar warna solid/gelap tidak terlihat "plastik" dan bebas *color banding* saat kompresi MP4.
- **Hairline Editorial Grid & Blueprint Callouts (DNA Video Referensi):**
  - Garis pemisah tajam `1px solid` (opacity 8–15%) dan label monospace kecil (`FIG. 01`, `LIVE SYNC`, garis ukur `366 PX × 240 PX`) di pojok kanvas untuk kesan *engineered*.
- **Apple-style Liquid Glass & Dark Tiltable Cards (DNA `labs-studio`):**
  - Gunakan *frosted backdrop blur*, border tipis dengan *top specular rim* (`linear-gradient` cahaya putih 25% di sisi atas ke 5% di bawah), dan *multi-layered shadow* (kombinasi shadow pendek pekat + shadow lebar lembut).
- **Pixel / Dot-Matrix Halftone Motif:**
  - Untuk menyatukan gaya *brand film* dengan produk, visualisasikan elemen analitik/AI (misal *Spending Pattern* atau *AI Insight*) menggunakan grid titik (*dot-matrix*) SVG/Canvas yang radius atau opasitasnya beranimasi secara prosedural mengikuti `frame`.

---

## 6. Pola "Money Shots" Khusus SaaS & Money Management

Gunakan pola berikut untuk Act 3 saat menyorot dashboard keuangan:
1. **Odometer / Number Roll:** Angka saldo utama atau *net worth* wajib beranimasi naik dari nilai awal ke nilai target memakai kurva *Expo Out* dengan `font-variant-numeric: tabular-nums` (agar lebar digit tidak bergetar).
2. **Progressive Cashflow Chart:** Gambar garis kurva SVG memakai `strokeDashoffset` dari penuh ke `0`, atau naikkan batang *bar chart* secara *staggered* (selisih 3 frame per batang), lalu kursor mampir ke puncak grafik untuk memunculkan *floating tooltip*.
3. **Financial Chain Reaction:** Tunjukkan kekuatan aplikasi dalam satu rantai aksi nyata:
   - Kursor klik tombol `+ Add Expense` (`frame X`, SFX: `click`)
   - Kartu transaksi baru meluncur masuk di daftar (`frame X+6`, SFX: `pop`)
   - *Progress bar* kategori budget langsung terisi mulus (`frame X+10`, SFX: `whoosh_soft`)
   - Angka *Total Balance* di kartu atas ikut terkalkulasi turun secara *odometer* (`frame X+12`, SFX: `tick_settle`)
