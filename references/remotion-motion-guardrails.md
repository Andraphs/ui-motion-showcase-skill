---
trigger: always_on
---

# Remotion UI Motion Showcase Guardrails

Setiap kali membuat, mengedit, atau me-review kode video di proyek Remotion ini, patuhi aturan berikut tanpa kecuali:

1. **Determinisme 100%:** Seluruh animasi wajib diturunkan secara murni dari `useCurrentFrame()`. Dilarang keras memakai CSS `transition`, CSS `@keyframes`, `Math.random()` mentah (wajib pakai `random(seed)` dari Remotion), `Date.now()`, atau `setTimeout`.
2. **FPS Terkunci 60:** Semua komposisi default menggunakan `fps={60}` untuk menjamin kelancaran *motion blur* dan *micro-interaction*.
3. **Anti-AI Slop:** Tolak *fade-in* polos (`opacity` saja), kurva *bouncy* berlebihan, kursor garis lurus tanpa jeda antisipasi, dan tampilan dashboard penuh tanpa *crop/zoom*.
4. **Tabular Numbers:** Semua angka keuangan, saldo, persentase, dan timer wajib menggunakan `fontVariantNumeric: 'tabular-nums'`.
5. **Verifikasi Visual Sebelum Render:** Setelah mengubah tata letak atau adegan baru, selalu tawarkan atau jalankan render *still frame* (`npx remotion still`) pada titik-titik kunci untuk memastikan tidak ada elemen tumpang tindih sebelum merender MP4 final.
6. **Sinkronisasi Audio:** Setiap penambahan interaksi tombol, kartu muncul, atau transisi kamera wajib memperbarui daftar `SFX_CUES` di `src/audio/cues.ts`.
