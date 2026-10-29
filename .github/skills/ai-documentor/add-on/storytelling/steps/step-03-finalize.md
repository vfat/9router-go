# Step 3: Finalize — Variations + Guidelines + Output

> Buat variasi cerita untuk channel berbeda, susun usage guidelines, dan generate output final.

---

## Input dari Step 2

```yaml
craft:
  story_beats: "..."
  character_voice: "..."
  conflict_tension: "..."
  transformation: "..."
  emotional_arc: "..."
  emotional_touchpoints: "..."
  opening_hook: "..."
  core_narrative: "..."
  complete_story: "..."
```

---

## Yang Dilakukan Agent

### 1. Create Story Variations

Tanya channel atau format apa yang akan menggunakan cerita ini.

Berdasarkan respons, buat:

1. **Short Version** (1-3 kalimat) — untuk social media, email subject lines, quick pitches
2. **Medium Version** (1-2 paragraf) — untuk email body, blog intro, executive summary
3. **Extended Version** (narasi lengkap) — untuk artikel, presentasi, case studies, website

**Checkpoint output:** `short_version`, `medium_version`, `extended_version`

---

### 2. Usage Guidelines

Tanya di mana dan bagaimana cerita akan digunakan.

Pertimbangkan dan tuliskan:

- **Best Channels** — channel terbaik untuk story type ini
- **Audience Considerations** — adaptasi spesifik per audiens
- **Tone & Voice Notes** — konsistensi tone dengan brand/konteks
- **Adaptation Suggestions** — visual, multimedia, atau enhancement lain

**Checkpoint output:** `best_channels`, `audience_considerations`, `tone_notes`, `adaptation_suggestions`

---

### 3. Refinement & Next Steps

Tanya:

- Bagian mana dari cerita yang paling kuat?
- Area mana yang perlu lebih di-refine?
- Apa resolusi kunci atau call to action untuk cerita Anda?
- Apakah perlu versi tambahan untuk audiens atau tujuan lain?
- Bagaimana Anda akan menguji cerita ini dengan audiens?

**Checkpoint output:** `resolution`, `refinement_opportunities`, `additional_versions`, `feedback_plan`

---

### 4. Generate Final Output

Kompilasi semua komponen cerita ke dalam template terstruktur.

Sebelum finalisasi:

1. Pastikan semua versi cerita lengkap dan terpolish
2. Format sesuai struktur template (`add-on/storytelling/template/story-template.md`)
3. Sertakan semua strategic guidance dan usage notes
4. Verifikasi konsistensi tone dan voice
5. Isi semua placeholder template dengan konten aktual

### 5. Write Output File

Tulis dokumen final ke `.ai-doc/storytelling/story-{date}-{topic-slug}.md`.

Jika folder `.ai-doc/storytelling/` belum ada, buat terlebih dahulu.

### 6. Konfirmasi ke User

```
✅ Story complete, {user_name}!

  📁 .ai-doc/storytelling/
  └── 📄 story-{date}-{topic-slug}.md

Framework: {framework_name} ({category})
Variasi: Short ✓ | Medium ✓ | Extended ✓
Channels: {best_channels ringkas}

Cerita siap digunakan! Anda bisa:
  - Edit langsung file output untuk penyesuaian
  - Jalankan sesi storytelling baru untuk cerita lain
  - Gunakan variasi short/medium sesuai channel target
```

Setelah selesai, **return kontrol ke core AI Documentor**.

---

## Output Step 3

```yaml
finalize:
  output_file: ".ai-doc/storytelling/story-{date}-{topic-slug}.md"
  short_version: "..."
  medium_version: "..."
  extended_version: "..."
  best_channels: "..."
  audience_considerations: "..."
  tone_notes: "..."
  adaptation_suggestions: "..."
  resolution: "..."
  refinement_opportunities: "..."
  additional_versions: "..."
  feedback_plan: "..."
  status: "complete"
```

---

## Aturan

- **JANGAN skip variasi** — minimal short + medium + extended harus terisi
- **JANGAN overwrite** file yang sudah ada — jika file output sudah ada, tambahkan suffix numerik
- **PASTIKAN** template terisi lengkap — tidak boleh ada placeholder `{{...}}` yang tertinggal
- **SIMPAN** semua output di `.ai-doc/storytelling/` — jangan di tempat lain
- **KEMBALIKAN** kontrol ke core AI Documentor setelah selesai
