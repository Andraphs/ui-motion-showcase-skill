---
trigger: always_on
---

# Remotion UI Motion Showcase Guardrails (v1.2 — Universal 9 Core Production Rules)

Aturan standar ini bersifat **brand-agnostic** (berlaku untuk semua produk SaaS, web app, maupun mobile app — *dark mode* maupun *light mode*). Patuhi seluruh poin berikut tanpa kecuali:

## A. Determinisme & Pondasi Teknis Remotion
1. **Determinisme 100%:** Seluruh animasi wajib diturunkan secara murni dari `useCurrentFrame()`. Dilarang memakai CSS `transition`, CSS `@keyframes`, `Math.random()` mentah (wajib `random(seed)`), `Date.now()`, atau `setTimeout`.
2. **FPS Terkunci 60 & Tabular Numbers:** Semua komposisi default memakai `fps={60}`. Semua angka dinamis (saldo, skor, persentase, timer) wajib memakai `fontVariantNumeric: 'tabular-nums'`.
3. **Anti-AI Slop:** Tolak *fade-in* polos (`opacity` saja), kurva *bouncy* berlebihan, dan tampilan halaman penuh tanpa *crop/zoom*.

## B. 9 Core Production Rules (Brand-Agnostic)
1. **Zero Dead-Air & Post-Action Hold Limit:** Batas maksimal *post-action hold* adalah **20–30 frame (~0,35–0,5 detik)** dengan **continuous subtle camera drift** (`scale: 1.0 → 1.04` atau `rotateX/Y` maks `1°–2°`). Jika adegan bertahan **> 90 frame (1,5 detik)** (misal *outro CTA*), WAJIB memicu **secondary micro-interaction** (kursor mendekat *hover*, *border glow*, atau *light sheen*).
2. **Zero-Latency Cursor Entry:** Kursor wajib mulai masuk maksimal **15–30 frame (0,25–0,5 detik)** setelah transisi pembuka selesai, meluncur lewat jalur Bezier dan tiba di target dalam **35–50 frame (~0,6–0,8 detik)**, jeda *hover* **4–6 frame**, lalu klik (`scale: 0.92` selama **3 frame**).
3. **Theme-Locked Color Rotation:** Rotasikan latar antar-adegan tanpa memecah polaritas produk — **Dark-Mode:** *Deep Neutral Base* (`#06050B`/`#09090B`) → *Architectural Tinted Grid* (`#131126`/`#0F172A` + grid `rgba(255,255,255,0.06)`) → *Full-Bleed Brand Accent*; **Light-Mode:** *Crisp Off-White* (`#F8FAFC`) → *Soft Brand-Tinted Grid* → *Full-Bleed Brand Accent*.
4. **Action-Driven Layout Shift (Center-First):** Mulai setiap modul UI utama tepat di **tengah kanvas (`x: 0`)**. Geser modul ke kiri (`translateX` *heavy deceleration*) hanya sebagai reaksi langsung dari klik kursor, bersamaan dengan *pop-out* kartu notifikasi/*insight* dari sisi kanannya (kiri ke kanan).
5. **Mathematical Hand-Off (Zero-Snap):** Nilai akhir `scale`, `rotateX`, `rotateY`, dan `translate` pada frame terakhir Scene A wajib **identik 100%** dengan frame pertama Scene B sebelum kamera bergerak menyesuaikan tata letak berikutnya.
6. **Audio Deduplication Lock (Anti-Double SFX):** Dilarang memasang `<Audio />` di dalam komponen anak yang di-render berulang (`.map()`/*loop* seperti teks *per-word*/*per-line*). Seluruh SFX dipicu terpusat di level *master timeline/beat* dengan jarak minimal **10 frame** antar-efek suara sejenis.
7. **Kinetic Headline + Reactive Glass Card Pairing:** Setiap adegan fitur wajib memadukan **1 Kinetic Headline eksternal** (3–5 kata, *masked line-rise* kurva *Expo Out* `[0.16, 1, 0.3, 1]`) dengan **1 Reactive Glass Card / Floating Insight Pill** (*backdrop-blur* + border `1px`) yang bereaksi langsung saat tombol diklik.
8. **Safe-Area & HUD Collision Guard:** Kunci zona aman **70–90 px** di tepi atas dan bawah khusus untuk *HUD metadata* (`FIG. 01`, indikator waktu) dan *Kinetic Headline*. Batasi UI utama dan *floating card* di *center stage* dengan jarak pemisah (*gap*) minimal **24–32 px** dari teks judul eksternal.
9. **Strict Frame-Budget Lock (Storyboard-First):** Dilarang menulis kode animasi sebelum menyusun dan menyetujui *Beat Sheet Storyboard* yang total durasinya **pas 100%** dari durasi target serta mengunci *Rentang Frame, Warna Latar, Gerakan Kamera, Frame Klik Kursor, Teks Kinetic, dan Timestamp SFX*.

## C. Verifikasi Wajib Sebelum Render Final
- Wajib render *still frames* (`npx remotion still`) — termasuk pasangan frame batas adegan untuk cek *Mathematical Hand-Off* (`zero-snap`), *safe-area* `70–90 px`, gap `24–32 px`, dan *center-first layout* — sebelum merender MP4 final.
