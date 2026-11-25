# Audio-Visual Lock & Procedural SFX Synthesis

Video UI tanpa *sound design* terasa 50% lebih datar. Setiap gerakan besar dan interaksi mikro wajib terkunci secara matematis ke nomor frame Remotion.

## 1. Sumber Audio (2 Layer Wajib)
1. **Background Music Bed (1 Trek Utuh):** Satu file musik instrumental (ambient/minimal electronic/lo-fi crisp) yang berjalan dari frame `0` hingga akhir sebagai "lem" transisi. Volume default `0.28–0.35`, dengan *fade-out* mulus di 30–45 frame terakhir.
2. **Procedural Tactile SFX (Per Frame Cue):** File `.wav` pendek yang digenerate secara prosedural langsung dari kode (tanpa mencari file MP3 berlisensi).

## 2. Format Pemetaan `src/audio/cues.ts`
Selalu definisikan seluruh ketukan audio dalam satu sumber kebenaran (*single source of truth*) agar sinkron dengan animasi visual:

```ts
export type SfxType = 'whoosh' | 'whoosh_soft' | 'click' | 'pop' | 'tick' | 'impact_sub';

export interface SfxCue {
  frame: number;
  type: SfxType;
  volume: number;
  label: string;
}

export const SFX_CUES: SfxCue[] = [
  { frame: 12,  type: 'whoosh',      volume: 0.55, label: 'Logo geometric slide' },
  { frame: 255, type: 'click',       volume: 0.70, label: 'CTA button pressed' },
  { frame: 260, type: 'impact_sub',  volume: 0.50, label: 'Zoom into dashboard' },
  { frame: 410, type: 'pop',         volume: 0.60, label: 'New expense card enters' },
  { frame: 416, type: 'whoosh_soft', volume: 0.40, label: 'Budget bar fills' },
];
```

## 2B. Aturan Cooldown & Anti-Stacking SFX (Revisi #6)

Untuk mencegah suara ganda (*double-trigger*), *audio phasing*, atau bunyi menumpuk yang terasa berisik:
1. **Jeda Cooldown Minimal 10 Frame:** Dua cue dengan `type` SFX yang sama—atau dalam grup frekuensi serupa (`whoosh` & `whoosh_soft`, atau `click` & `tick`)—WAJIB berjarak minimal **10 frame** (`~167ms` pada 60fps).
2. **Prioritas Aksi Utama:** Jika dua animasi terjadi dalam rentang `< 10` frame (misal kartu bergeser bersamaan dengan klik tombol), bunyikan **hanya 1 SFX terpenting** (aksi pemicu utamanya), atau geser aksi sekunder minimal `+10` frame.
3. **Validasi Otomatis di `src/audio/cues.ts`:** Sertakan fungsi validator ringan (atau cek manual sebelum render) yang memverifikasi tidak ada dua `SfxCue` bertipe sama dengan selisih `Math.abs(a.frame - b.frame) < 10`.

## 3. Generator SFX Prosedural Murni Node.js (`scripts/generate-sfx.mjs`)
Jika workspace belum memiliki `sfx.py` dari Motion-By-Labs, buat dan jalankan skrip Node.js mandiri ini (`node scripts/generate-sfx.mjs`) untuk men-generate paket lengkap `.wav` (44.1kHz, 16-bit PCM) ke `public/sfx/` tanpa dependency tambahan:

- `click.wav` — *Tactile mechanical UI click* (transient 18ms, pitch sweep cepat 1800Hz → 420Hz + sedikit *filtered noise*).
- `pop.wav` — *Bubble/card pop* lembut (65ms, sine sweep naik 280Hz → 640Hz dengan envelope eksponensial).
- `whoosh.wav` — *Camera/editorial swipe* (280ms, *bandpass-filtered white noise* dengan modulasi frekuensi naik-turun).
- `whoosh_soft.wav` — *Micro slide / progress bar* (160ms, lebih lembut dan frekuensi lebih rendah).
- `tick.wav` — *Odometer / toggle subtle tick* (12ms, impuls kayu/mekanis halus).
- `impact_sub.wav` — *Deep low-end transition hit* (320ms, sine sub-bass 110Hz → 42Hz untuk momen masuk ke dashboard atau logo lockup).

*(Catatan: Jika pengguna sudah memasang folder `motion-graphics/scripts/sfx.py` dari repo Motion-By-Labs dan Python 3 tersedia, agen juga diperbolehkan memanfaatkan `sfx.py` untuk variasi tekstur suara yang lebih kaya.)*
