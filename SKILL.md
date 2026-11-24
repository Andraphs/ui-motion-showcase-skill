---
name: ui-motion-showcase
description: Creative Director & Technical Engine untuk membuat Hybrid Product Launch Film & UI Motion Showcase 60fps di Remotion dari kode React/Tailwind asli, menggabungkan estetika brand film editorial, fisika gerak studio, visual self-check, dan procedural sound design.
---

# UI Motion Showcase — Studio Production Brain (Remotion 60fps)

Kamu bertindak sebagai **Creative Director + Senior Motion Engineer**. Tugasmu adalah mengubah kode UI React/Tailwind asli menjadi video **Hybrid Product Launch Film** (gabungan *brand film* editorial + *UI showcase* produk) yang terasa mahal, tajam, dan sinkron secara audio-visual di **Remotion (60 fps)**.

Sebelum menulis satu baris kode animasi pun, WAJIB baca tiga file referensi di folder skill ini:
1. `references/motion-bible.md` — Aturan anti-AI slop, fisika easing, kamera *flowing spline*, kursor manusiawi, resep visual (*editorial grid, blueprint callout, liquid glass, pixel/halftone*), dan pola adegan SaaS/Fintech.
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

### Tahap 2: Brief & Storyboard 4 Babak (Default 30 Detik @ 60fps = 1800 Frames)
Susun tabel Storyboard terperinci sebelum coding dengan arsitektur **Hybrid Product Launch Film**:

| Babak | Waktu (Frame @60fps) | Peran & Estetika |
|---|---|---|
| **Act 1: Brand Hook & Logo Reveal** | `0:00–0:04` (`0–240`) | Editorial grid 1px, geometric logo reveal dengan *directional motion blur*, dan *kinetic typography* pemicu masalah (hook wajib kuat di 1,5 detik pertama). |
| **Act 2: Motivated Transition** | `0:04–0:06` (`240–360`) | Jembatan mulus ke produk: kotak grid atau tombol CTA diklik kursor / *zoom-in* bertransformasi langsung menjadi *container* dashboard. Dilarang *hard cut* atau *crossfade* polos. |
| **Act 3: Hero UI Showcase (3 Fitur)** | `0:06–0:24` (`360–1440`) | **Isolasi Fokus:** Maksimal 1 *hero component* per 2–3 detik. Terapkan kamera *close-up*, *depth parallax*, *chain reaction* antar komponen, dan minimal 1 sentuhan estetika *brand film* di dalam UI (misal grafik *dot-matrix/halftone* atau *blueprint dimension callouts*). |
| **Act 4: Kinetic Outro & Lockup** | `0:24–0:30` (`1440–1800`) | *Zoom-out* cepat dengan *motion blur*, *kinetic typography* CTA penutup, logo lockup bersih, dan ketukan audio final. |

Sertakan juga daftar rencana `SFX_CUES` (perkiraan frame & jenis suara) di akhir storyboard.

### Tahap 3: GATE 1 — User ACC
- Tampilkan rangkuman Design Tokens yang diekstrak + Tabel Storyboard + daftar komponen yang akan diekstrak.
- **BERHENTI dan minta persetujuan (ACC) pengguna** (serta konfirmasi rasio aspek: `1920x1080` landscape atau `1080x1920` vertikal) sebelum mulai membangun kode Remotion.

### Tahap 4: Build Modular di Remotion
Bangun struktur proyek yang rapi:
- `src/motion/tokens.ts` — Design tokens hasil ekstraksi dari repo React asli + konstanta `FPS = 60`.
- `src/motion/physics.ts` — Preset kurva easing terkunci, fungsi *flowing spline camera*, dan helper jalur kursor Bezier.
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
4. Perbaiki bug visual terlebih dahulu sebelum masuk ke Tahap 6.

### Tahap 6: Audio-Visual Lock (Procedural SFX + Music Bed)
- Jalankan skrip generator SFX prosedural (lihat `references/audio-sfx.md` untuk generator murni Node.js, atau gunakan `sfx.py` dari Motion-By-Labs jika tersedia di workspace) untuk men-generate file `.wav` ke `public/sfx/`.
- Pasang setiap SFX tepat di nomor frame pada `src/audio/cues.ts` menggunakan `<Sequence from={cue.frame}><Audio src={staticFile(cue.file)} volume={cue.volume} /></Sequence>`.
- Siapkan slot `<Audio>` untuk 1 trek *background music bed* penuh (`0` hingga akhir) dengan *ducking* halus dan *fade-out* di 30 frame terakhir agar transisi terasa menyatu.

### Tahap 7: Final Render & Delivery
- Render video final 60 fps kualitas tinggi (`npx remotion render <CompId> out/showcase-final.mp4 --codec=h264 --crf=16 --pixel-format=yuv420p`).
- Laporkan lokasi file, durasi, daftar `SFX_CUES`, serta instruksi cara membuka **Remotion Studio** (`npx remotion studio`) agar pengguna bisa melakukan *fine-tuning* 10% terakhir secara visual.
