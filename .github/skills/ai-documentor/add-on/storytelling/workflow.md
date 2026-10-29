# Storytelling Add-On — Workflow

> Orchestrator / entry point untuk add-on storytelling AI Documentor.
> Panggil workflow ini ketika user ingin membuat narasi terstruktur melalui storytelling.

---

## Overview

```
User meminta "storytelling", "buatkan cerita", "narrative", "story"
        │
        ▼
┌────────────────────────────────────────┐
│  Storytelling Add-On                   │
│  (setup → craft → finalize)           │
│          │                             │
│  ┌───────┼───────────────┐             │
│  ▼       ▼               ▼             │
│ Setup   Craft           Finalize       │
│ Context Narrative       Variations     │
│ + Pick  + Emotional     + Guidelines   │
│ Framework  Arc          + Output       │
│         + Hook                         │
│         + Draft                        │
└────────────────────────────────────────┘
```

### Output

| Output | Lokasi | Isi |
|---|---|---|
| **Story Document** | `.ai-doc/storytelling/story-{date}-{topic-slug}.md` | Dokumen narasi lengkap dengan semua variasi |

### Template

| Template | Lokasi |
|---|---|
| **Story Template** | `add-on/storytelling/template/story-template.md` |

---

## Flow Eksekusi

Gunakan 3 step berikut secara berurutan:

```text
Step 1: Setup (Context + Framework)
  ├── Cek apakah context data diberikan bersama invocation
  ├── Jika ya: load context, tanya angle/emphasis
  ├── Jika tidak: tanya purpose, audience, key messages, constraints
  ├── Load story types dari add-on/method/story/README.md
  ├── Jika persona aktif: auto-recommend story types berdasarkan persona
  ├── Presentasikan framework options per kategori
  ├── User pilih framework (atau minta rekomendasi)
  └── HALT → konfirmasi pilihan framework

Step 2: Craft (Elements + Arc + Hook + Draft)
  ├── Gather story elements via Socratic method
  ├── Framework-specific guided questions
  ├── Develop emotional arc (awal, turning point, akhir)
  ├── Craft opening hook
  ├── Pilih mode: user draft / agent draft / co-create
  ├── Write core narrative
  └── HALT → review draft bersama user

Step 3: Finalize (Variations + Guidelines + Output)
  ├── Create story variations: short / medium / extended
  ├── Usage guidelines: channels, audience, tone
  ├── Refinement & next steps
  ├── Generate final output ke .ai-doc/storytelling/
  └── Selesai → return kontrol ke core AI Documentor
```

---

## Facilitation Principles

Prinsip-prinsip yang harus aktif sepanjang sesi storytelling:

1. **Guide, don't write** — tanya dulu, draft kalau diminta
2. **Find the conflict** — setiap cerita butuh tension/struggle
3. **Show, don't tell** — gunakan detail vivid dan konkret
4. **Change is central** — cerita tanpa transformasi bukan cerita
5. **Emotion drives memory** — temukan feeling yang tepat
6. **Authenticity resonates** — jaga suara asli user

---

## Integrasi Persona

Jika persona aktif saat sesi storytelling:

1. Baca `story-method` dari persona `customize.toml`
2. Auto-recommend story types yang sesuai persona
3. Persona mempengaruhi gaya fasilitasi dan pertanyaan panduan
4. Mapping persona → story types tersedia di `add-on/method/story/README.md` section "Berdasarkan Persona"

Saat add-on storytelling membutuhkan persona, ia akan membaca:
1. `.ai-doc/personas/list.md` — daftar persona yang tersedia
2. `.ai-doc/personas/<nama>/customize.toml` — source of truth konfigurasi (termasuk `story-method`)
3. `.ai-doc/personas/<nama>/persona.md` — detail karakter & komunikasi

---

## Behavioral Constraints

- Komunikasikan semua respons dalam bahasa yang sesuai `communication_language` dari config
- Jangan berikan estimasi waktu
- Setelah setiap checkpoint, tampilkan konten yang di-generate, lalu presentasikan opsi:
  - `[c] Continue` — lanjut ke step berikutnya
  - `[r] Revise` — revisi bagian saat ini
  - `[y] YOLO` — auto-complete sampai akhir
- Tunggu respons user sebelum lanjut

---

## Referensi File

| File | Kegunaan |
|---|---|
| `add-on/storytelling/steps/step-01-setup.md` | Detail langkah setup context + pilih framework |
| `add-on/storytelling/steps/step-02-craft.md` | Detail langkah craft narrative |
| `add-on/storytelling/steps/step-03-finalize.md` | Detail langkah finalize + output |
| `add-on/storytelling/template/story-template.md` | Template output story |
| `add-on/method/story/README.md` | Katalog 30 story types (6 kategori) |
