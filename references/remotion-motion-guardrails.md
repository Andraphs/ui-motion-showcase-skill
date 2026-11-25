---
trigger: always_on
---

# Remotion UI Motion Showcase Guardrails (v1.1 — Universal)

Setiap kali membuat, mengedit, atau me-review kode video di proyek Remotion ini, patuhi seluruh aturan berikut tanpa kecuali:

1. **Determinisme 100%:** Seluruh animasi wajib diturunkan secara murni dari `useCurrentFrame()`. Dilarang keras memakai CSS `transition`, CSS `@keyframes`, `Math.random()` mentah (wajib pakai `random(seed)` dari Remotion), `Date.now()`, atau `setTimeout`.
2. **FPS Terkunci 60:** Semua komposisi default menggunakan `fps={60}` untuk menjamin kelancaran *motion blur* dan *micro-interaction*.
3. **Zero Dead-Air & Continuous Drift:** Batasi jeda (*hold*) pasca-aksi maksimal **20–30 frame** dan wajibkan *subtle continuous camera drift* (1–2%) agar tidak ada frame yang benar-benar diam.
4. **Zero-Latency Cursor Entry:** Pada adegan interaktif, kursor wajib mulai masuk maksimal **15–30 frame** setelah transisi adegan selesai (menggunakan kurva Bezier + jeda hover 4–6 frame sebelum klik).
5. **Polarity-Aware Palette Rotation:** Rotasi warna latar belakang antar adegan wajib konsisten dengan polaritas *design tokens* produk (*Dark-Spectrum* untuk produk dark mode; *Light-Spectrum* untuk produk light mode).
6. **Center-First Layout Shift:** Setiap *Hero Module* wajib tampil di tengah kanvas terlebih dahulu sebelum bergeser halus memberi ruang bagi *pop-out* notifikasi atau *Reactive Glass Insight Card*.
7. **Zero-Snap Scene Continuity:** Nilai akhir transformasi (`scale`, `translate`, `rotate`) pada Scene A wajib disinkronkan persis dengan nilai awal Scene B agar tidak pernah terjadi lompatan visual (*snap*).
8. **External Kinetic Headline + Reactive Glass Cards:** Setiap fitur utama di Act 3 wajib dipadukan dengan *External Kinetic Headline* dan *Reactive Glass Insight Card* yang merespons interaksi UI.
9. **HUD & UI Safe-Area (`≥ 80px`):** Pertahankan jarak aman minimal **80px dari tepi atas dan tepi bawah** kanvas untuk seluruh HUD, callout, headline, dan UI (atau lebih besar pada format 9:16 untuk overlay platform).
10. **Strict Frame Budget Before Coding:** Dilarang menulis kode scene sebelum menyusun tabel alokasi durasi frame yang totalnya tepat sama dengan durasi komposisi.
11. **Audio-Visual Lock & SFX Cooldown (`≥ 10f`):** Setiap aksi utama wajib tercatat di `src/audio/cues.ts`, dengan jeda minimal **10 frame** antar SFX bertipe sama/serupa untuk mencegah suara ganda.
12. **Anti-AI Slop & Tabular Numbers:** Tolak *fade-in* polos (`opacity` saja), kurva *bouncy* berlebihan, dan tampilan halaman penuh tanpa *crop/zoom*. Semua angka dinamis wajib memakai `fontVariantNumeric: 'tabular-nums'`.
13. **Verifikasi Visual Sebelum Render:** Wajib render *still frames* (`npx remotion still`)—termasuk pasangan frame batas adegan untuk cek *zero-snap*—sebelum merender MP4 final.
