# UI Motion Showcase Skill

Skill untuk **Antigravity** yang berperan sebagai *Senior UI/UX Engineer*, *Design System Architect*, sekaligus *Motion Director* untuk mengubah kode aplikasi web/SaaS (React/Tailwind/HTML) menjadi video **UI Motion Showcase 60fps** berstandar studio di **Remotion** — tanpa memerlukan 3D berat atau Blender.

Skill ini menggabungkan **Voice-First pacing** (terintegrasi alur kerja ElevenLabs), koreografi kamera **2.5D**, *kinetic typography*, rotasi warna latar adaptif, *visual self-check*, *3-layer audio mixing*, hingga render supersampling HiDPI (2K/4K 60fps).

## Isi Skill

```text
ui-motion-showcase/
├── SKILL.md
└── references/
    └── production-and-audio-rules.md
```

- **`SKILL.md`** — Alur produksi 6 tahap berurutan (*Audit Kode & Design Tokens* → *Voice-First Script siap copy-paste ElevenLabs & Storyboard Gate* → *Build Terpusat di Remotion* → *Visual Self-Check & Zero Layout Shift* → *3-Layer Audio Lock, Auto Re-Timing & Mixing* → *Final Render 60fps Supersampling 2K/4K*).
- **`references/production-and-audio-rules.md`** — **20 Hukum Produksi Universal 2.5D** (*anti-AI slop*), mencakup fisika gerak & kamera 2.5D, transisi *shape-morph* & *color-flood*, rotasi warna latar adaptif (*Dark/Light Mode*), koreografi kursor, *Action-Driven Layout Shift*, *Safe-Area Guard*, disiplin SFX terdeduplikasi, *Zero DOM Reflow*, *1:1 Component Proportion Lock*, *In-Place Zoom & Content-Populated Morph*, rotasi layout tipografi, hingga *Vector Text Sharpness Lock*.

## Cara Memasang

Pasang skill ini ke direktori global skill Antigravity di `~/.gemini/config/skills/ui-motion-showcase` (lokasi global aktif Antigravity):

### Menggunakan Git (Direkomendasikan)

**macOS / Linux:**
```bash
mkdir -p ~/.gemini/config/skills
git clone https://github.com/Andraphs/ui-motion-showcase-skill.git ~/.gemini/config/skills/ui-motion-showcase
```

**Windows (PowerShell):**
```powershell
New-Item -ItemType Directory -Force -Path "$HOME\.gemini\config\skills"
git clone https://github.com/Andraphs/ui-motion-showcase-skill.git "$HOME\.gemini\config\skills\ui-motion-showcase"
```

### Pemasangan Manual
1. Unduh atau *clone* repository ini.
2. Salin `SKILL.md` dan folder `references/` ke:
   - macOS/Linux: `~/.gemini/config/skills/ui-motion-showcase/`
   - Windows: `%USERPROFILE%\.gemini\config\skills\ui-motion-showcase\`
3. Mulai sesi baru di Antigravity agar skill `ui-motion-showcase` otomatis terbaca.
