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
- **Aksi cepat (14–22 frame / 0.23–0.36 detik)** → diikuti **Post-Action Hold ketat (maks 20–30 frame / ~0.35–0.50 detik)** dengan *continuous subtle camera drift* agar mata penonton sempat mencerna angka/pesan tanpa layar membeku (*dead-air*).

---

## 2B. Zero Dead-Air, Post-Action Hold Limit & Zero-Latency Cursor Entry (Core Rules #1 & #2)

- **Zero Dead-Air & Post-Action Hold Limit:** Dilarang membiarkan layar membeku diam (`0%` pergerakan) setelah animasi UI atau klik kursor selesai. Batas maksimal *post-action hold* sebelum transisi berikutnya adalah **20–30 frame (~0,35–0,5 detik)**.
- **Continuous Subtle Camera Drift:** Terapkan *drift* halus di sepanjang setiap adegan—misalnya zoom-in perlahan `scale: 1.0 → 1.04` atau kemiringan 3D halus `rotateX/Y` maksimal `1°–2°`.
- **Secondary Micro-Interaction untuk Adegan Panjang (`> 90f`):** Jika sebuah adegan (seperti *outro CTA*) harus bertahan lebih dari **90 frame (1,5 detik)**, WAJIB memicu *secondary micro-interaction* (kursor mendekat untuk *hover*, *border glow* menyala, atau sapuan kilau cahaya/*light sheen*).
- **Zero-Latency Cursor Entry:** Kursor wajib mulai memasuki frame maksimal **15–30 frame (0,25–0,5 detik)** setelah transisi pembuka adegan selesai, bergerak via *Bezier curve* hingga tiba di target dalam **35–50 frame (~0,6–0,8 detik)**, berhenti sejenak **4–6 frame** saat *hover state* aktif, lalu melakukan klik (`scale: 0.92` selama **3 frame**).

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

## 3B. Mathematical Hand-Off & Theme-Locked Color Rotation (Core Rules #3 & #5)

### Mathematical Hand-Off (Zero-Snap Scene Transitions)
Transisi *zoom-in* atau perpindahan antar-komposisi tidak boleh menimbulkan patahan (*snap*) posisi, skala, atau sudut rotasi:
- Saat menyambungkan Scene A (misalnya *Launcher/Portal*) ke Scene B (misalnya *Feature Canvas*), nilai akhir properti `scale`, `rotateX`, `rotateY`, dan `translate` pada frame terakhir Scene A **wajib identik 100%** dengan nilai awal Scene B sebelum kamera bergerak menyesuaikan tata letak judul fitur.
- Simpan seluruh nilai transisi bersama di konstanta terpusat (`src/motion/handoff.ts`) agar perubahan di satu adegan tidak menimbulkan patahan di adegan berikutnya.

### Theme-Locked Color Rotation (Polarity-Consistent Palette)
Rotasikan warna latar belakang antar-adegan untuk mencegah kejenuhan visual, tetapi **wajib mengunci polaritas tema produk** (jangan memaksakan latar putih polos pada produk *dark-mode*, atau sebaliknya):
- **Untuk Produk Dark-Mode:** Rotasikan latar di antara 3 spektrum gelap-ke-aksen:
  1. *Deep Neutral Base* (misal `#06050B` / `#09090B`)
  2. *Architectural Tinted Grid* (warna gelap bernuansa brand seperti `#131126` atau `#0F172A` dengan garis grid tipis `rgba(255,255,255,0.06)`)
  3. *Full-Bleed Brand Accent* (warna aksen utama produk secara penuh dengan teks/HUD kontras tinggi)
- **Untuk Produk Light-Mode:** Rotasikan latar di antara *Crisp Off-White (`#F8FAFC`)* → *Soft Brand-Tinted Grid* → *Full-Bleed Brand Accent*.

---

## 4. Kursor Manusiawi (Human Cursor Protocol)

Kursor AI yang bergerak lurus kaku langsung merusak ilusi video studio. Wajib terapkan siklus 4 fase ini:
1. **Curved Approach:** Kursor mulai masuk maksimal **15–30 frame** pasca-transisi, lalu bergerak dari titik A ke target tombol B menggunakan kurva **Quadratic Bezier** (offset lengkungan 40–90px, bukan garis lurus) dengan kurva *Expo Out*, tiba di target dalam **35–50 frame (~0,6–0,8 detik)**.
2. **Hover & Anticipation Pause:** Saat kursor menyentuh area tombol, langsung aktifkan *hover state* (border lebih terang / background shift) lalu **JEDA 4–6 frame** (seolah mata manusia sedang memastikan tombolnya).
3. **Tactile Click:** Kompres tombol & kursor (`scale: 0.92` selama **3 frame**), lalu lepas kembali ke `1.0`. Kunci frame tepat saat klik ke `SFX_CUES` (`click`).
4. **Consequence (Chain Reaction):** 2–4 frame setelah klik, picu perubahan UI (*Action-Driven Layout Shift*, *Reactive Glass Card* keluar dari sisi kanan, atau angka terkalkulasi ulang).

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

## 5B. Action-Driven Layout Shift, Kinetic Headline + Glass Card & Safe-Area Guard (Core Rules #4, #7 & #8)

### Action-Driven Layout Shift (Center-First Choreography)
Modul UI utama harus selalu dimulai dalam posisi seimbang di tengah layar; pergeseran tata letak hanya boleh terjadi sebagai **reaksi langsung** dari klik kursor:
- Saat adegan dimulai, posisikan kartu/modul UI utama tepat di **tengah kanvas (`x: 0`)** tanpa menyisakan ruang kosong yang canggung di salah satu sisi.
- Tepat setelah kursor mengklik tombol aksi utama, geser modul utama ke sisi kiri (`translateX` dengan kurva *heavy deceleration* / `SNAPPY_UI` spring) bersamaan dengan kemunculan kartu notifikasi/*insight* yang meluncur keluar dari balik sisi kanannya (**arah kiri ke kanan / pop-out**).

### Kinetic Headline + Reactive Glass Card Pairing
Jangan bergantung pada judul kecil bawaan antarmuka aplikasi; setiap adegan fitur wajib memadukan tipografi eksternal dengan umpan balik visual yang reaktif:
1. **External Kinetic Headline:** Setiap adegan fitur wajib memiliki **1 Kinetic Headline eksternal** (3–5 kata tajam tanpa klise AI, animasi *masked line-rise* dengan kurva *Expo Out* `[0.16, 1, 0.3, 1]`).
2. **Reactive Glass Card / Floating Insight Pill:** Sertakan **1 Reactive Glass Card / Floating Insight Pill** (efek *backdrop-blur* + border tipis `1px` dengan *top specular rim*) yang menampilkan perubahan angka, status, atau *progress bar* secara langsung begitu kursor mengeklik tombol aksi.

### Safe-Area & HUD Collision Guard
Elemen tipografi eksternal dan *HUD metadata* tidak boleh menutupi angka utama, grafik, atau tombol interaksi di dalam komponen UI:
- Kunci zona aman tepi kanvas sebesar **70–90 px** di bagian atas dan bawah khusus untuk *HUD metadata* (`FIG. 01`, indikator waktu) serta *Kinetic Headline*.
- Batasi area komponen UI utama dan *floating card* di dalam zona tengah (*center stage*) dengan jarak pemisah (*gap*) minimal **24–32 px** dari teks judul eksternal.
- *(Khusus rasio vertikal 9:16 untuk TikTok/Reels/Shorts: perbesar margin bawah menjadi `≥ 180 px` dan atas `≥ 120 px` untuk menghindari overlay tombol platform).*

---

## 6. Pola "Hero Shots" Universal (Untuk Semua Jenis Produk UI)

Sesuaikan pola interaksi di Act 3 dengan domain produk yang diaudit dari repo React/desain:
1. **Live Data & Metric Roll:** Angka utama (saldo, konversi, kecepatan, analitik, skor) beranimasi naik/turun menggunakan kurva *Expo Out* dengan `fontVariantNumeric: 'tabular-nums'`.
2. **Progressive Visualization:** Grafik garis/area (`strokeDashoffset`), *bar chart* berurutan (*stagger 3 frame*), atau *dot-matrix/halftone* tergambar otomatis lalu memunculkan *tooltip* saat disentuh kursor.
3. **Interactive Chain Reaction:** Satu aksi kursor (klik tombol, toggle, input perintah, atau pilih filter) memicu reaksi berantai dalam 3–12 frame: aksi klik (`click`) → *Center-First Layout Shift* → *Reactive Glass Insight Card* / notifikasi masuk (`pop`) → komponen data utama diperbarui (`whoosh_soft` / `tick`).
