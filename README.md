# UI Motion Showcase Skill

Skill untuk **Antigravity** yang berperan sebagai *Creative Director* sekaligus *Senior Motion Engineer* untuk mengubah kode UI React/Tailwind asli menjadi video **Hybrid Product Launch Film & UI Motion Showcase 60fps** di **Remotion**.

Skill ini menggabungkan estetika *brand film* editorial, fisika gerak studio (*anti-AI slop*), verifikasi visual otomatis (*contact sheet*), dan *procedural sound design* yang terkunci presisi ke nomor frame.

## Isi Skill

```text
ui-motion-showcase/
├── SKILL.md
└── references/
    ├── motion-bible.md
    ├── audio-sfx.md
    └── remotion-motion-guardrails.md
```

- **`SKILL.md`** — Otak produksi utama berisi **7-Stage Gated Pipeline** (Audit Design Tokens → Storyboard 4 Babak + *Mandatory Frame Budget* → Gate ACC → Build Modular Remotion → Visual Self-Check + cek *safe-area* & *zero-snap* → Audio-Visual Lock → Final Render 60fps).
- **`references/motion-bible.md`** — Aturan *anti-AI slop*, kurva easing & fisika `spring()` 60fps, pacing *zero dead-air*, *zero-snap continuity*, *polarity-aware palette rotation*, *center-first layout shift*, *safe-area* `≥ 80px`, protokol kursor manusiawi, dan pola *Hero Shots* universal untuk semua jenis produk UI.
- **`references/audio-sfx.md`** — Protokol *Audio-Visual Lock*, pemetaan `SFX_CUES`, aturan *cooldown* SFX minimal 10 frame (*anti-stacking*), serta panduan generator efek suara `.wav` prosedural murni Node.js.
- **`references/remotion-motion-guardrails.md`** — Guardrails v1.1 (Universal, 13 aturan) untuk determinisme 60fps, *zero dead-air*, *zero-snap*, *safe-area*, dan checklist anti-bug Remotion (disalin ke `.agents/rules/` saat membuat proyek Remotion baru).

## Cara Memasang

Pasang skill ini ke direktori global skill Antigravity di `~/.gemini/antigravity/skills/ui-motion-showcase`:

### Menggunakan Git (Direkomendasikan)

**macOS / Linux:**
```bash
mkdir -p ~/.gemini/antigravity/skills
git clone https://github.com/Andraphs/ui-motion-showcase-skill.git ~/.gemini/antigravity/skills/ui-motion-showcase
```

**Windows (PowerShell):**
```powershell
New-Item -ItemType Directory -Force -Path "$HOME\.gemini\antigravity\skills"
git clone https://github.com/Andraphs/ui-motion-showcase-skill.git "$HOME\.gemini\antigravity\skills\ui-motion-showcase"
```

### Pemasangan Manual
1. Unduh atau *clone* repository ini.
2. Salin seluruh isi folder (beserta `SKILL.md` dan folder `references/`) ke:
   - macOS/Linux: `~/.gemini/antigravity/skills/ui-motion-showcase/`
   - Windows: `%USERPROFILE%\.gemini\antigravity\skills\ui-motion-showcase\`
3. Mulai sesi baru di Antigravity agar skill `ui-motion-showcase` otomatis terbaca.
