# Production, Visual & Audio Rules (Universal 2.5D Standard)

Seluruh aturan teknis di bawah ini wajib diterapkan pada setiap video UI motion showcase agar terhindar dari animasi generik ("AI slop") dan bug tata letak.

---

## A. Koreografi Kamera & Fisika Gerak (2.5D)
1. **Kurva Gerak Konsisten (Anti-AI Slop):** Dilarang menggunakan `ease-in-out` standar, `fade-in` polos, atau efek mantul (*bouncy*) murahan pada UI. Gunakan kurva *heavy deceleration* (cepat di awal, mendarat mulus tanpa memantul, seperti `cubic-bezier(0.16, 1, 0.3, 1)` atau `spring` dengan `damping: 200`).
2. **Arah Kamera Tidak Pernah Berbalik:** Jangan melakukan *zoom-in* lalu *zoom-out* pada subjek yang sama dalam satu adegan. Setiap gerakan kamera harus mengalir dalam satu kurva kontinu.
3. **Zero Dead-Air & Post-Action Hold Limit:**
   - Batas maksimal layar diam setelah aksi kursor selesai adalah **20–30 frame (~0,35–0,5 detik)**.
   - Selalu berikan **continuous subtle camera drift** (`scale: 1.0 → 1.04` atau kemiringan 3D `1°–2°`) di sepanjang adegan fitur.
   - Jika adegan penutup (*outro CTA*) bertahan lebih dari **90 frame (1,5 detik)**, wajib memicu *secondary micro-interaction* (kursor melakukan *hover* ke tombol CTA, *border glow* menyala, atau sapuan *light sheen*).
4. **Hentikan Drift di Frame Akhir:** Khusus pada 1–1,5 detik terakhir setelah logo lockup terkunci, hentikan pergerakan kamera dan posisikan logo tepat di tengah kanvas berdasarkan *visible bounding box*-nya.

## B. Transisi & Rotasi Warna Latar
5. **Shape-Morph & Color-Flood Transitions:** Hindari *cross-fade* polos antar-babak. Gunakan elemen kunci dari adegan sebelumnya (kartu, tombol, atau kotak grid) yang melakukan antisipasi (*~8% squash*), membesar memenuhi frame sambil membanjiri kanvas dengan warna latar babak berikutnya, lalu memunculkan UI baru (`scale: 0.93 → 1.0`).
6. **Mathematical Hand-Off (Zero-Snap):** Saat menyambungkan transisi *zoom* dari Scene A ke Scene B, nilai akhir `scale`, `rotateX/Y`, dan `translate` di frame terakhir Scene A wajib identik 100% dengan nilai awal Scene B.
7. **Theme-Locked Color Rotation:** Rotasikan warna latar antar-babak untuk menyegarkan mata penonton tanpa merusak identitas polaritas produk:
   - **Produk Dark-Mode:** Rotasikan antara *Pure Obsidian (`#06050B`)* → *Architectural Tinted Grid* (warna gelap bernuansa brand + garis grid `3–3.5 px` opasitas rendah agar tidak *shimmer* saat zoom) → *Full-Bleed Brand Accent*.
   - **Produk Light-Mode:** Rotasikan antara *Crisp Off-White (`#F8FAFC`)* → *Soft Brand-Tinted Grid* → *Full-Bleed Brand Accent*.

## C. Koreografi UI & Kursor
8. **Zero-Latency Cursor Entry:** Kursor wajib mulai memasuki frame maksimal **15–30 frame (0,25–0,5 detik)** setelah transisi adegan selesai. Gerakkan melengkung (*Bezier*), tiba di target dalam `35–50 frame`, beri jeda antisipasi **4–6 frame** saat *hover* menyala, lalu klik (`scale: 0.92` selama 3 frame).
9. **Action-Driven Layout Shift (Center-First):** Modul UI utama wajib muncul di **tengah kanvas (`x: 0`)** pada awal adegan. Baru *setelah* kursor mengeklik tombol aksi, modul utama bergeser mulus ke sisi kiri (`translateX`) bersamaan dengan kartu notifikasi/*insight* yang muncul dari balik sisi kanannya (*pop-out* kiri ke kanan).
10. **Safe-Area & HUD Collision Guard:** Kunci zona aman tepi atas dan bawah sebesar **75–90 px** khusus untuk *HUD metadata* (`FIG. 01`, timer) dan *Kinetic Headline*. Pertahankan jarak (*gap*) minimal **24–32 px** antara judul eksternal dengan kartu UI di tengah.

## D. Tipografi Kinetik & Kartu Reaktif
11. **Kinetic Headline + Reactive Glass Card:** Setiap adegan fitur wajib memadukan:
    - **1 Kinetic Headline eksternal** (3–5 kata kunci dari narasi VO, tracking rapat `-3%`, muncul per kata dari *blur ke tajam* dengan kenaikan vertikal halus dan stagger `4–5 frame` @60fps).
    - **1 Reactive Glass Card / Insight Pill** (`backdrop-filter: blur()`, border `1px`) yang angka atau *progress bar*-nya berubah seketika merespons klik kursor.
12. **Aturan Kontras Gradien Teks:** Teks dengan efek gradien hanya boleh digunakan di atas latar belakang gelap (*Obsidian/Dark Grid*). Di atas latar belakang warna aksen solid atau latar terang, wajib gunakan warna teks solid kontras tinggi.

## E. Disiplin Audio & SFX
13. **Audio Deduplication Lock:** Dilarang memasang komponen `<Audio />` di dalam elemen yang di-render berulang (*loop/map* per-kata atau per-baris). Seluruh SFX wajib dipicu secara terpusat dari daftar `EVENTS` dengan jarak minimal **10 frame** antar-suara sejenis.
14. **Pemetaan SFX Berdasarkan Makna:**
    - **Whoosh (halus, low-pass):** Hanya untuk gerakan fisik (kamera bergerak atau kartu bergeser).
    - **Pop / Tonal Click / Soft Hit:** Untuk potongan gambar (*cut*), perubahan bentuk (*morph*), atau munculnya *insight pill*.
    - **Tactile Click:** Tepat pada frame klik kursor.
    - **Larangan:** Hindari suara bel/ding murahan, suara ketikan keyboard, atau dengungan (*hum*) panjang. Variasikan *pitch* (`0.92–1.08`) jika efek suara yang sama dipakai lebih dari sekali.

## F. Stabilitas Layout, Proporsi, Tipografi & Ketajaman Render
15. **Zero DOM Reflow (Pre-Allocated Fixed Layout):** Dilarang menggunakan *conditional mounting* (`{frame >= X && ...}`) di dalam kontainer `flex` dinamis untuk memunculkan elemen secara bertahap. Alokasikan seluruh ukuran dan koordinat final elemen sejak frame pertama adegan (atau gunakan `position: absolute` / tinggi wadah tetap untuk judul atas), lalu kontrol kemunculannya murni dengan `opacity`, `clip-path`, dan `transform` agar elemen lain tidak bergeser atau berderak.
16. **1:1 Component Proportion Lock:** Saat membangun ulang tampilan *Overview Dashboard*, kunci tinggi kartu, padding, dan proporsi header 1:1 sesuai kode asli aplikasi. Dilarang memakai `flex-1` atau `height: 100%` yang membuat kartu bawah memanjang secara vertikal.
17. **In-Place Zoom & Content-Populated Morph:** Saat melakukan *zoom-in* pada satu kartu dashboard untuk masuk ke scene detail, kunci porosnya di posisi awal (`transformOrigin: 'center center'`) tanpa pergeseran horizontal (`translateX`). Saat kartu ber-morph menjadi kartu baru, pastikan ukuran akhir (`width`, `height`, `borderRadius`) 100% identik dengan kartu tujuan dan mulai munculkan konten di dalamnya sejak titik tengah transisi (`50%`) agar tidak pernah tampil sebagai kotak kosong.
18. **Typography Layout Rotation & Inline Micro-Elements:** Dilarang memakai pola judul rata tengah di atas kartu secara berulang di setiap scene. Variasikan komposisi tipografi antar-babak dengan memadukan *Asymmetric Left-Right Split*, *Inline UI Badge/Icon di dalam kalimat* (contoh: `Deep Work. [⚡] 12ms Sync.`), *Animated Highlight Pill*, dan *3-Beat Staccato*.
19. **Cursor Entry Pacing & Single-Click Discipline:** Pada scene *Overview Dashboard*, tunda kemunculan kursor ~1,5–2 detik agar penonton dapat melihat keseluruhan UI terlebih dahulu sebelum kursor meluncur masuk dan melakukan **tepat 1 kali klik taktil** (`scale: 1.0 -> 0.86 -> 1.0` + 1 click ripple). Pada scene *Outro*, setelah kursor melakukan hover ke tombol CTA, pertahankan kursor tetap diam di tombol tersebut dengan sapuan *light sheen* hingga frame terakhir.
20. **Vector Text Sharpness Lock:** Pastikan skala akhir (*resting scale*) kartu UI berhenti tepat di `scale: 1.0` (gunakan rentang masuk `0.94 -> 1.0`, hindari `> 1.0` yang meregangkan tekstur bitmap Chromium), batasi sudut `rotateX/Y` maksimal `1deg` pada tampilan dashboard padat, dan pastikan animasi `filter: blur(...)` berakhir dengan `filter: 'none'` saat mencapai `0px`.
