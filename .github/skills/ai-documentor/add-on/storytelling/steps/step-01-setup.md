# Step 1: Setup — Context + Framework Selection

> Kumpulkan konteks cerita dan pilih storytelling framework yang tepat.
> Langkah ini membangun fondasi untuk seluruh sesi storytelling.

---

## Input

- **Context data** (opsional): Bisa diberikan bersama invocation add-on
- **Persona** (opsional): Jika persona aktif, baca `story-method` dari `customize.toml`

---

## Yang Dilakukan Agent

### 1. Cek Context Data

**Jika context data diberikan:**

- Load dokumen context dari path yang diberikan
- Pelajari background information, brand details, atau subject matter
- Akui tujuan storytelling yang spesifik
- Tanya: "Saya lihat kita akan membuat cerita berdasarkan konteks yang diberikan. Angle atau penekanan apa yang Anda inginkan?"

**Jika tidak ada context data:**

- Lakukan context gathering interaktif
- Tanya:
  - Apa tujuan cerita ini? (marketing, pitch, brand narrative, case study, presentasi, dll)
  - Siapa target audiens Anda?
  - Apa pesan kunci atau takeaway yang ingin audiens dapatkan?
  - Adakah batasan? (panjang, tone, medium, brand guidelines)
- Tunggu respons user. Konteks ini menentukan pendekatan naratif.

**Checkpoint output:** `story_purpose`, `target_audience`, `key_messages`, `constraints`

---

### 2. Load Story Frameworks

Baca katalog story types dari `add-on/method/story/README.md`.

Parse data dengan field: `category`, `story_type`, `name`, `description`, `key_questions`.

### 3. Presentasikan Framework Options

Berdasarkan konteks dari langkah 1, presentasikan opsi framework per kategori:

**Transformation Narratives:**
1. **Hero's Journey** — Classic transformation arc with adventure and return
2. **Pixar Story Spine** — Emotional structure building tension to resolution
3. **Customer Journey** — Before/after transformation narrative
4. **Challenge Overcome** — Dramatic obstacle-to-victory structure
5. **Character Arc** — Personal evolution through experience

**Strategic Narratives:**
6. **Brand Story** — Values, mission, and unique positioning
7. **Vision Narrative** — Future-focused aspirational story
8. **Origin Story** — Foundational narrative of how it began
9. **Positioning Story** — Unique market position and differentiation
10. **Culture Story** — Organizational values and identity

**Persuasive Narratives:**
11. **Pitch Narrative** — Persuasive problem-to-solution structure
12. **Sales Story** — Customer-centric value demonstration
13. **Change Story** — Case for transformation and mobilization
14. **Fundraising Story** — Emotionally connecting donor to mission
15. **Advocacy Story** — Galvanizing support for cause or movement

**Analytical Narratives:**
16. **Data Storytelling** — Transform insights into compelling narrative
17. **Case Study** — Detailed real-world application and results
18. **Research Narrative** — Research findings in accessible way
19. **Insight Narrative** — Reveal non-obvious truth or pattern
20. **Process Story** — Behind-the-scenes how something was made

**Emotional Narratives:**
21. **Hook Driven** — Powerful opening and emotional touchpoints
22. **Conflict Resolution** — Tension building and satisfying resolution
23. **Empathy Story** — Emotional connection and perspective-taking
24. **Human Interest** — Universal human experiences
25. **Vulnerable Story** — Authentic sharing of struggle and truth

**Conversational Narratives:**
26. **Free Association** — Ide melompat-lompat tanpa filter
27. **Thought Experiment** — "What if..." eksplorasi murni
28. **Analogi & Metaphor** — Jelaskan lewat perbandingan sehari-hari
29. **Devil's Advocate Ringan** — Tantang asumsi secara santai
30. **Reflective Listening** — Paraphrase untuk deepen understanding

Tanya framework mana yang paling cocok. Terima nomor `1-30` atau permintaan rekomendasi.

### 4. Handle Rekomendasi

Jika user minta rekomendasi:

- Analisis `story_purpose`, `target_audience`, dan `key_messages`
- Jika persona aktif, prioritaskan story types dari `story-method` persona
- Rekomendasikan framework terbaik dengan rasionale jelas
- Format: "Berdasarkan {story_purpose} Anda untuk {target_audience}, saya merekomendasikan **{framework_name}** karena {rationale}"

### 5. Konfirmasi Framework

Setelah user memilih, konfirmasi:

```
✅ Framework dipilih: {framework_name}
   Kategori: {category}
   Tujuan: {story_purpose}
   Audiens: {target_audience}

Lanjut ke tahap pembuatan narasi?
```

HALT — tunggu konfirmasi user.

---

## Output Step 1

```yaml
setup:
  story_purpose: "..."
  target_audience: "..."
  key_messages: "..."
  constraints: "..."
  story_type: "hero-journey"
  framework_name: "Hero's Journey"
  category: "transformation"
  persona_active: true/false
  persona_name: "..."
  status: "framework_selected"
```
