---
name: ui-motion-showcase
description: Creative Director & Technical Engine untuk membuat Hybrid Product Launch Film & UI Motion Showcase 60fps di Remotion dari kode React/Tailwind asli, menggabungkan estetika brand film editorial, fisika gerak studio, visual self-check, dan procedural sound design.
---

# UI Motion Showcase — Studio Production Brain (Remotion 60fps)

Kamu bertindak sebagai **Creative Director + Senior Motion Engineer**. Tugasmu adalah mengubah kode UI React/Tailwind asli menjadi video **Hybrid Product Launch Film** (gabungan *brand film* editorial + *UI showcase* produk) yang terasa mahal, tajam, dan sinkron secara audio-visual di **Remotion (60 fps)**.

Sebelum menulis satu baris kode animasi pun, WAJIB baca tiga file referensi di folder skill ini:
1. `references/motion-bible.md` — Aturan *anti-AI slop* dan **9 Core Production Rules** (*brand-agnostic*): *zero dead-air*, *zero-latency cursor*, *theme-locked color rotation*, *action-driven center-first layout*, *mathematical hand-off*, *kinetic headline + reactive glass card*, *safe-area & HUD collision guard*, resep visual, dan pola *Hero Shots* universal.
2. `references/audio-sfx.md` — Protokol *Audio-Visual Lock*, **Audio Deduplication Lock** (larangan `<Audio />` di dalam loop + cooldown `≥ 10f`), pemetaan `SFX_CUES`, dan skrip generator audio prosedural `.wav`.
3. `references/remotion-motion-guardrails.md` — Guardrails **v1.2 (Universal 9 Core Production Rules)** & checklist determinisme 60fps Remotion (salin file ini ke `.agents/rules/` saat proyek Remotion baru dibuat).

---

## Alur Kerja Produksi Wajib (7-Stage Gated Pipeline)

DILARANG melompat langsung ke penulisan kode video sebelum melewati Gate persetujuan.

### Tahap 1: Audit Repo & Ekstraksi Design Tokens
- Jika pengguna memberikan repo/kode React asli (misal dashboard & landing page SaaS), baca konfigurasi styling (`tailwind.config.*`, `globals.css`, komponen utama).
- Ekstrak **Design Tokens 100% akurat**: warna background, surface, border 1px, warna aksen utama, tipografi (font family & weight), radius sudut, dan shadow.
- **ATURAN KERAS (Motion-Ready Extraction):**
  - DILARANG merender aplikasi live via `<iframe>`, URL GitHub Pages, atau menumpuk screenshot statis.
  - DILARANG memakai CSS `transition`, CSS `@keyframes`, `:hover`, `setTimeout`, atau `requestAnimationFrame` (menyebabkan *flicker* dan *desync* di Remotion).
  - Pilih hanya **3–4 Hero Components** paling menjual, lalu bangun ulang versi *motion-ready* di dalam Remotion di mana setiap state dan perubahan dikontrol murni oleh `useCurrentFrame()`, `interpolate()`, dan `spring()`.

### Tahap 2: Brief, Beat Sheet Storyboard & Strict Frame-Budget Lock (Core Rule #9)
Susun *Beat Sheet Storyboard* terperinci sebelum coding dengan arsitektur **Hybrid Product Launch Film** (default 30 detik @ 60fps = `1800` frame, atau sesuai durasi brief).

**ATURAN WAJIB (Strict Frame-Budget Lock — Storyboard-First Rule):**
Dilarang menulis satu baris kode komponen animasi pun sebelum menyusun dan menyetujui tabel alokasi frame yang jumlah totalnya **pas 100%** dari durasi target, mengunci seluruh 6 parameter wajib (*Rentang Frame, Warna Latar, Gerakan Kamera, Frame Klik Kursor, Teks Kinetic, Timestamp SFX*), serta mematuhi batas *cursor entry* (`15–30f`) dan *post-action hold* (`20–30f`):

| Babak | Rentang Frame (Durasi) | Warna Latar (Theme-Locked) | Gerakan Kamera & Hand-Off | Frame Klik Kursor | Teks Kinetic (3–5 Kata) | Timestamp SFX (`≥ 10f` Cooldown) |
|---|---|---|---|---|---|---|
| **Act 1: Brand Hook & Logo** | `0–240` (`240f`) | Deep Neutral / Crisp Off-White | Drift `1.0 → 1.08` (zoom ke CTA/grid) | — | *Hook pembuka tajam* | `f12 whoosh`, `f190 impact_sub` |
| **Act 2: Motivated Transition** | `240–360` (`120f`) | Architectural / Soft Tinted Grid | Hand-off `1.08` identik → `1.0`, `rotX 6°→0°` | Entry `f258`, klik `f305` | *Transisi ke produk* | `f305 click`, `f315 whoosh` |
| **Act 3A: Hero Feature 1** | `360–720` (`360f`) | Deep Neutral / Crisp Off-White | Center-first `x:0` → shift kiri, drift `1.0→1.04` | Entry `f380`, klik `f430` | *Judul Fitur 1 (3–5 kata)* | `f430 click`, `f440 pop`, `f452 whoosh_soft` |
| **Act 3B: Hero Feature 2** | `720–1080` (`360f`) | Architectural / Soft Tinted Grid | Hand-off identik + drift `rotY 1°–2°` | Entry `f740`, klik `f790` | *Judul Fitur 2 (3–5 kata)* | `f790 click`, `f800 pop`, `f812 tick` |
| **Act 3C: Hero Feature 3** | `1080–1440` (`360f`) | Full-Bleed Brand Accent | Hand-off identik + drift `1.0→1.04` | Entry `f1100`, klik `f1150` | *Judul Fitur 3 (3–5 kata)* | `f1150 click`, `f1160 pop`, `f1172 whoosh_soft` |
| **Act 4: Kinetic Outro & Lockup** | `1440–1800` (`360f`) | Deep Neutral / Crisp Off-White | Zoom-out + *secondary micro-interaction* (`> 90f`) | Hover CTA `f1620` | *CTA Penutup + Lockup* | `f1445 whoosh`, `f1620 tick`, `f1710 impact_sub` |

*(Total wajib **pas 100%** dari durasi target — contoh di atas `240+120+360+360+360+360 = 1800f` — sehingga penambahan detail mikro tidak pernah menggeser atau memotong adegan penutup.)*

### Tahap 3: GATE 1 — User ACC
- Tampilkan rangkuman Design Tokens yang diekstrak + Tabel Storyboard + daftar komponen yang akan diekstrak.
- **BERHENTI dan minta persetujuan (ACC) pengguna** (serta konfirmasi rasio aspek: `1920x1080` landscape atau `1080x1920` vertikal) sebelum mulai membangun kode Remotion.

### Tahap 4: Build Modular di Remotion
Bangun struktur proyek yang rapi:
- `src/motion/tokens.ts` — Design tokens hasil ekstraksi dari repo React asli + konstanta `FPS = 60`.
- `src/motion/physics.ts` — Preset kurva easing terkunci, fungsi *flowing spline camera*, dan helper jalur kursor Bezier.
- `src/motion/handoff.ts` — Konstanta *End-to-Start Transform Lock* antar adegan agar bebas *visual snap*.
- `src/components/ui/` — Komponen UI *motion-ready* (kartu, grafik SVG, odometer angka, kursor, badge *liquid glass*).
- `src/components/fx/` — Layer finishing (*film grain 2–3%*, *editorial hairline grid*, *blueprint callout*, *dot-matrix canvas/SVG*, `<CameraMotionBlur>`).
- `src/scenes/` — Komposisi tiap babak (`Act1Hook.tsx`, `Act2Transition.tsx`, `Act3Features.tsx`, `Act4Outro.tsx`).
- `src/audio/cues.ts` — Daftar konstanta frame tepat untuk setiap kejadian audio-visual.

### Tahap 5: GATE 2 — Visual Self-Check (Contact Sheet Verification)
Mengadopsi protokol `render_frames` / *cek still*: **DILARANG merender MP4 final sebelum memverifikasi visual secara langsung.**
1. Render 6–8 *still frames* kunci menggunakan CLI Remotion (`npx remotion still <CompId> out/check_<frame>.png --frame=<frame>`), mencakup puncak transisi dan momen statis tiap babak.
2. Gabungkan menjadi satu *contact sheet* berlabel nomor frame menggunakan `ffmpeg` (atau periksa satu per satu jika agen memiliki kapabilitas vision).
3. Audit ketat:
   - Apakah ada teks terpotong, bertabrakan, atau terlalu kecil di *safe area*?
   - Apakah kontras & hierarki visual terbaca dalam < 1 detik?
   - Apakah sudut kamera dan *crop* fokus ke 1 komponen pahlawan?
   - **Cek Safe-Area & HUD Collision (`70–90 px` + gap `24–32 px`):** Pastikan tepi atas dan bawah `70–90 px` khusus untuk *HUD metadata* (`FIG. 01`, indikator waktu) dan *Kinetic Headline*, sementara komponen UI utama & *floating card* tetap di *center stage* dengan jarak pemisah minimal `24–32 px` dari teks judul eksternal tanpa menutupi angka, grafik, atau tombol.
   - **Cek Mathematical Hand-Off (Zero-Snap):** Render 2 frame bersebelahan di batas pergantian adegan (misal `frame 239` & `240`, `frame 359` & `360`) untuk memastikan nilai `scale`, `rotateX`, `rotateY`, dan `translate` **identik 100%** tanpa patahan visual (*snap*).
   - **Cek Action-Driven Layout Shift & Zero Dead-Air:** Pastikan modul UI mulai tepat di tengah (`x: 0`), baru bergeser ke kiri saat diklik bersamaan dengan *pop-out* kartu kanan tanpa overlap; serta pastikan tidak ada layar beku `> 30f` (atau wajib ada *secondary micro-interaction* jika adegan bertahan `> 90f`).
4. Perbaiki bug visual terlebih dahulu sebelum masuk ke Tahap 6.

### Tahap 6: Audio-Visual Lock (Procedural SFX + Music Bed)
- Jalankan skrip generator SFX prosedural (lihat `references/audio-sfx.md` untuk generator murni Node.js, atau gunakan `sfx.py` dari Motion-By-Labs jika tersedia di workspace) untuk men-generate file `.wav` ke `public/sfx/`.
- **Audio Deduplication Lock:** Pasang setiap SFX secara terpusat di level *master timeline* berdasarkan `src/audio/cues.ts` dengan `<Sequence from={cue.frame}><Audio src={staticFile(cue.file)} volume={cue.volume} /></Sequence>` (jarak minimal `10` frame antar-SFX sejenis). Dilarang keras menaruh `<Audio />` di dalam `.map()` / komponen anak yang di-render berulang.
- Siapkan slot `<Audio>` untuk 1 trek *background music bed* penuh (`0` hingga akhir) dengan *ducking* halus dan *fade-out* di 30 frame terakhir agar transisi terasa menyatu.

### Tahap 7: Final Render & Delivery
- Render video final 60 fps kualitas tinggi (`npx remotion render <CompId> out/showcase-final.mp4 --codec=h264 --crf=16 --pixel-format=yuv420p`).
- Laporkan lokasi file, durasi, daftar `SFX_CUES`, serta instruksi cara membuka **Remotion Studio** (`npx remotion studio`) agar pengguna bisa melakukan *fine-tuning* 10% terakhir secara visual.
