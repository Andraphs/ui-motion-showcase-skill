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
- **Aksi cepat (14–22 frame / 0.23–0.36 detik)** → diikuti **Hold/Breathing Room ketat (maks 20–30 frame / 0.33–0.50 detik)** dengan *continuous camera drift* 1–2% agar mata penonton sempat mencerna angka/pesan tanpa terasa berhenti (*dead-air*).

---

## 2B. Zero Dead-Air, Pacing Ketat & Cursor Latency (Revisi #1 & #2)

- **Zero Dead-Air Rule:** Batasi waktu jeda (*hold*) pasca-aksi maksimal **20–30 frame** (`0.33–0.50` detik pada 60fps). Dilarang membiarkan layar diam statis melebihi 30 frame.
- **Continuous Subtle Camera Drift:** Bahkan saat momen *hold* (20–30 frame), kamera WAJIB tetap memiliki *drift* halus sebesar **1–2%** (`scale` atau *pan* mikro terus berjalan) agar video selalu terasa hidup dan tidak pernah mengalami *dead-air*.
- **Zero-Latency Cursor Entry:** Pada setiap adegan yang melibatkan interaksi, kursor WAJIB mulai masuk ke kanvas maksimal **15–30 frame** setelah transisi pembuka adegan selesai. Jangan biarkan penonton menunggu kursor muncul di layar yang kosong.

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

## 3B. Zero-Snap Scene Continuity & Polarity-Aware Palette Rotation (Revisi #3 & #5)

### End-to-Start Transform Lock (Anti-Snap)
Setiap perpindahan dari Scene A ke Scene B WAJIB mensinkronkan nilai akhir transformasi Scene A dengan nilai awal Scene B:
- Kunci nilai `scale`, `translateX`, `translateY`, dan `rotateX/Y/Z` pada frame terakhir Scene A agar **identik atau berlanjut dalam satu kurva kecepatan** di frame pertama Scene B.
- Simpan nilai transisi bersama di konstanta terpusat (`src/motion/handoff.ts`) agar perubahan di satu adegan tidak menimbulkan lompatan (*snap*) di adegan berikutnya.

### Polarity-Aware Palette Rotation (Universal Dark & Light Mode)
Untuk menjaga variasi visual antar adegan tanpa merusak identitas produk, terapkan rotasi latar belakang berbasis polaritas *design tokens* produk yang sedang dikerjakan:
- **Jika produk bertema Dark Mode (*Dark-Spectrum Palette Rotation*):** Rotasikan latar belakang secara halus antar spektrum gelap—misalnya `Deep Base Dark` → `Dark Surface ber-tint 6–12% warna aksen produk` → `Deep Charcoal/Obsidian`. Dilarang melompat ke latar putih terang secara acak.
- **Jika produk bertema Light Mode (*Light-Spectrum Palette Rotation*):** Rotasikan latar belakang secara halus antar spektrum terang—misalnya `Warm Off-White / Pure Canvas` → `Soft Surface ber-tint 4–8% warna aksen produk` → `Clean Neutral`. Dilarang membalik ke latar hitam pekat kecuali untuk *Act 1/4* yang memang dimotivasi brand identity.

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

## 5B. Layout Architecture, Kinetic Headline + Glass Cards & Safe-Area (Revisi #4, #7 & #8)

### Center-First Layout Shift
- Setiap *Hero Module* (kartu utama, panel fitur, atau jendela UI) WAJIB masuk dan terkunci **di tengah kanvas (*center-aligned*) terlebih dahulu** sebagai titik fokus utama.
- Saat elemen sekunder muncul (*pop-out* notifikasi, panel detail, atau kartu *insight*), geser *Hero Module* secara halus (*layout shift* dengan `SNAPPY_UI` spring) ke kiri atau kanan untuk memberi ruang—bukan langsung ditaruh berat sebelah sejak frame pertama.

### External Kinetic Headline + Reactive Glass Insight Cards
Di setiap fitur pada Act 3, kombinasikan dua elemen naratif ini di luar bingkai UI utama:
1. **External Kinetic Headline:** Judul fitur singkat (2–5 kata tajam, tanpa jargon klise AI) yang diposisikan di luar modul UI dan masuk dengan *masked rise* (`clip-path` + `translateY`).
2. **Reactive Glass Insight Card:** Kartu bergaya *frosted/liquid glass* (border `1px` dengan *top specular rim*) yang muncul setelah *Center-First Layout Shift* dan **bereaksi langsung** terhadap aksi UI (misalnya status berubah, metrik terhitung ulang, atau badge konfirmasi menyala 2–4 frame setelah klik).

### HUD & UI Safe-Area Margin (`≥ 80px`)
- Tetapkan zona aman (*safe-area*) wajib minimal **`80px` dari tepi atas** dan **`80px` dari tepi bawah** kanvas (pada resolusi `1920x1080` maupun `1080x1920`).
- Seluruh HUD, label *blueprint callout* (`FIG. 01`, dimensi piksel), *External Kinetic Headline*, dan batas luar *Hero Module* DILARANG masuk ke dalam zona `80px` atas/bawah agar bebas tabrakan visual dan tidak terpotong pemutar video.
- *(Khusus rasio vertikal 9:16 untuk TikTok/Reels/Shorts: perbesar margin bawah menjadi `≥ 180px` dan atas `≥ 120px` untuk menghindari overlay tombol platform).*

---

## 6. Pola "Hero Shots" Universal (Untuk Semua Jenis Produk UI)

Sesuaikan pola interaksi di Act 3 dengan domain produk yang diaudit dari repo React/desain:
1. **Live Data & Metric Roll:** Angka utama (saldo, konversi, kecepatan, analitik, skor) beranimasi naik/turun menggunakan kurva *Expo Out* dengan `fontVariantNumeric: 'tabular-nums'`.
2. **Progressive Visualization:** Grafik garis/area (`strokeDashoffset`), *bar chart* berurutan (*stagger 3 frame*), atau *dot-matrix/halftone* tergambar otomatis lalu memunculkan *tooltip* saat disentuh kursor.
3. **Interactive Chain Reaction:** Satu aksi kursor (klik tombol, toggle, input perintah, atau pilih filter) memicu reaksi berantai dalam 3–12 frame: aksi klik (`click`) → *Center-First Layout Shift* → *Reactive Glass Insight Card* / notifikasi masuk (`pop`) → komponen data utama diperbarui (`whoosh_soft` / `tick`).
