---
name: ui-motion-showcase
description: Creative Director & Technical Engine untuk membuat Hybrid Product Launch Film & UI Motion Showcase 60fps di Remotion dari kode React/Tailwind asli, menggabungkan estetika brand film editorial, fisika gerak studio, visual self-check, dan procedural sound design.
---

# UI Motion Showcase — Studio Production Brain (Remotion 60fps)

Kamu bertindak sebagai **Creative Director + Senior Motion Engineer**. Tugasmu adalah mengubah kode UI React/Tailwind asli menjadi video **Hybrid Product Launch Film** (gabungan *brand film* editorial + *UI showcase* produk) yang terasa mahal, tajam, dan sinkron secara audio-visual di **Remotion (60 fps)**.

Sebelum menulis satu baris kode animasi pun, WAJIB baca tiga file referensi di folder skill ini:
1. `references/motion-bible.md` — Aturan anti-AI slop, fisika easing, pacing *zero dead-air*, kamera *flowing spline*, *zero-snap continuity*, *polarity-aware palette rotation*, kursor manusiawi, *center-first layout*, *safe-area*, resep visual, dan pola *Hero Shots* universal.
2. `references/audio-sfx.md` — Protokol *Audio-Visual Lock*, pemetaan `SFX_CUES`, dan skrip generator audio prosedural `.wav`.
3. `references/remotion-motion-guardrails.md` — Guardrails determinisme 60fps & checklist anti-bug Remotion (salin file ini ke `.agents/rules/` saat proyek Remotion baru dibuat).

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

### Tahap 2: Brief, Storyboard 4 Babak & Mandatory Frame Budget (Revisi #9)
Susun Storyboard terperinci sebelum coding dengan arsitektur **Hybrid Product Launch Film** (default 30 detik @ 60fps = `1800` frame, atau sesuai durasi brief).

**ATURAN WAJIB (Strict Frame Budget Allocation):**
Sebelum menulis satu baris kode pun, agen WAJIB menyusun **Tabel Alokasi Durasi Frame** yang totalnya presisi 100% dengan durasi video, mencakup batas *cursor entry* (maks `15–30` frame pasca-transisi) dan batas *hold* pasca-aksi (maks `20–30` frame):

| Scene / Babak | Range Frame | Durasi (f) | Polaritas BG | Cursor Entry | Hold Budget | Hand-off Transform (Akhir → Awal Berikutnya) |
|---|---:|---:|---|---:|---:|---|
| **Act 1: Brand Hook & Logo** | `0–240` | `240` | Base Spectrum | — | `24f` | `scale: 1.0 → 1.08` (zoom ke CTA/grid) |
| **Act 2: Motivated Transition** | `240–360` | `120` | Tint Shift 1 | `f+18` | `20f` | `scale: 1.08 → 1.0`, `rotX: 6° → 0°` |
| **Act 3A: Hero Feature 1** | `360–720` | `360` | Base Spectrum | `f+20` | `25f` | `scale: 1.02 → 1.02`, `x: -140px → 0` |
| **Act 3B: Hero Feature 2** | `720–1080` | `360` | Tint Shift 2 | `f+18` | `25f` | `scale: 1.04 → 1.04`, `rotY: -4° → 0°` |
| **Act 3C: Hero Feature 3** | `1080–1440` | `360` | Base Spectrum | `f+20` | `24f` | `scale: 1.05 → 1.05`, `rotX: 0° → 5°` |
| **Act 4: Kinetic Outro & Lockup** | `1440–1800` | `360` | Deep/Clean Base | — | `30f` | Final settle (`scale: 1.0`) |

Sertakan daftar rencana `SFX_CUES` di bawah tabel (wajib mematuhi jeda cooldown minimal `10` frame antar tipe SFX serupa).

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
   - **Cek Safe-Area (`≥ 80px`):** Pastikan tidak ada HUD, *blueprint callout*, *kinetic headline*, atau komponen UI yang menyentuh zona `80px` teratas atau terbawah kanvas.
   - **Cek Zero-Snap Continuity:** Render 2 frame bersebelahan di batas pergantian adegan (misal `frame 239` & `240`, `frame 359` & `360`) untuk memastikan tidak ada lompatan skala, rotasi, atau posisi (*visual snap*).
   - **Cek Center-First & No Overlap:** Pastikan *Hero Module* mulai dari posisi tengah dan tidak menabrak *External Kinetic Headline* maupun *Reactive Glass Card* saat bergeser.
4. Perbaiki bug visual terlebih dahulu sebelum masuk ke Tahap 6.

### Tahap 6: Audio-Visual Lock (Procedural SFX + Music Bed)
- Jalankan skrip generator SFX prosedural (lihat `references/audio-sfx.md` untuk generator murni Node.js, atau gunakan `sfx.py` dari Motion-By-Labs jika tersedia di workspace) untuk men-generate file `.wav` ke `public/sfx/`.
- Pasang setiap SFX tepat di nomor frame pada `src/audio/cues.ts` menggunakan `<Sequence from={cue.frame}><Audio src={staticFile(cue.file)} volume={cue.volume} /></Sequence>`.
- Siapkan slot `<Audio>` untuk 1 trek *background music bed* penuh (`0` hingga akhir) dengan *ducking* halus dan *fade-out* di 30 frame terakhir agar transisi terasa menyatu.

### Tahap 7: Final Render & Delivery
- Render video final 60 fps kualitas tinggi (`npx remotion render <CompId> out/showcase-final.mp4 --codec=h264 --crf=16 --pixel-format=yuv420p`).
- Laporkan lokasi file, durasi, daftar `SFX_CUES`, serta instruksi cara membuka **Remotion Studio** (`npx remotion studio`) agar pengguna bisa melakukan *fine-tuning* 10% terakhir secara visual.
