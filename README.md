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

- **`SKILL.md`** — Otak produksi utama berisi **7-Stage Gated Pipeline** dan **Strict Frame-Budget Lock** (*Beat Sheet Storyboard* pas 100%, audit *safe-area* `70–90 px`, *mathematical hand-off*, hingga render 60fps).
- **`references/motion-bible.md`** — Panduan *anti-AI slop* dan **9 Core Production Rules** (*brand-agnostic*, Dark & Light Mode): *zero dead-air* + *secondary micro-interaction* (`> 90f`), *zero-latency cursor* (`15–30f`), *theme-locked color rotation*, *action-driven center-first layout shift*, *mathematical hand-off*, *kinetic headline + reactive glass card*, *safe-area* `70–90 px` + *gap* `24–32 px`, dan pola *Hero Shots* universal.
- **`references/audio-sfx.md`** — Protokol *Audio-Visual Lock*, **Audio Deduplication Lock** (larangan `<Audio />` di dalam loop + *master timeline* cooldown `≥ 10f`), serta panduan generator efek suara `.wav` prosedural murni Node.js.
- **`references/remotion-motion-guardrails.md`** — Guardrails **v1.2 (Universal 9 Core Production Rules)** untuk determinisme 60fps dan checklist anti-bug Remotion (disalin ke `.agents/rules/` saat membuat proyek Remotion baru).

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
