# Step 2: Craft — Elements + Arc + Hook + Draft

> Kumpulkan elemen narasi, bangun emotional arc, craft opening hook, dan tulis draft cerita.
> Ini adalah tahap kreatif utama dalam sesi storytelling.

---

## Input dari Step 1

```yaml
setup:
  story_purpose: "..."
  target_audience: "..."
  key_messages: "..."
  story_type: "hero-journey"
  framework_name: "Hero's Journey"
  category: "transformation"
```

---

## Yang Dilakukan Agent

### 1. Gather Story Elements (Socratic Method)

Gunakan metode Socratic — **tanya, jangan tulis** kecuali user minta. Draw out the story melalui pertanyaan.

Prinsip aktif:
- Setiap cerita butuh **conflict/tension** — temukan struggle-nya
- **Show, don't tell** — gunakan detail vivid dan konkret
- **Change is essential** — tanya apa yang bertransformasi
- **Emotion drives memory** — temukan feeling-nya
- **Authenticity** — jaga suara asli user

Berdasarkan framework yang dipilih, gunakan pertanyaan panduan yang sesuai.

#### Framework-Specific Questions

**Hero's Journey:**
- Siapa atau apa hero dalam cerita ini?
- Seperti apa dunia biasa mereka sebelum petualangan?
- Panggilan apa yang mengganggu dunia mereka?
- Tantangan atau trial apa yang mereka hadapi?
- Bagaimana mereka bertransformasi oleh perjalanan ini?
- Kebijaksanaan apa yang mereka bawa pulang?

**Pixar Story Spine:**
- Once upon a time, apa situasinya?
- Every day, apa rutinitasnya?
- Until one day, apa yang berubah?
- Because of that, apa yang terjadi selanjutnya?
- And because of that? (lanjutkan rantai)
- Until finally, bagaimana diselesaikan?

**Customer Journey:**
- Apa perjuangan sebelumnya?
- Momen penemuan apa yang terjadi?
- Bagaimana mereka mengimplementasikannya?
- Transformasi apa yang terjadi?
- Apa realitas baru mereka?

**Challenge Overcome:**
- Hambatan apa yang menghalangi kemajuan?
- Bagaimana stakes meningkat?
- Apa momen tergelap?
- Terobosan apa yang terjadi?
- Apa yang dipelajari?

**Character Arc:**
- Siapa mereka di awal?
- Apa yang memaksa perubahan?
- Apa yang mereka tolak?
- Terobosan apa yang menggeser mereka?
- Siapa mereka sekarang?

**Brand Story:**
- Apa spark awal brand ini?
- Nilai inti apa yang mendorong setiap keputusan?
- Bagaimana dampaknya pada pelanggan/user?
- Apa yang membuatnya berbeda dari alternatif?
- Ke mana arahnya di masa depan?

**Pitch Narrative:**
- Seperti apa landscape masalahnya?
- Apa visi Anda untuk solusinya?
- Bukti atau traction apa yang memvalidasi pendekatan ini?
- Tindakan apa yang Anda inginkan dari audiens?

**Data Storytelling:**
- Konteks apa yang audiens butuhkan?
- Apa revelasi data atau insight kuncinya?
- Pola apa yang menjelaskan insight ini?
- So what? Mengapa ini penting?
- Tindakan apa yang harus dipicu oleh insight ini?

**Story types lain:** Gunakan `key_questions` dari `add-on/method/story/README.md` untuk framework yang dipilih.

**Checkpoint output:** `story_beats`, `character_voice`, `conflict_tension`, `transformation`

---

### 2. Develop Emotional Arc

Setelah elemen terkumpul, bangun perjalanan emosional:

Tanya:
- Emosi apa yang harus audiens rasakan di **awal**?
- Pergeseran emosional apa yang terjadi di **turning point**?
- Emosi apa yang harus mereka **bawa pulang** di akhir?
- Di mana **puncak emosional** (ketegangan tinggi atau kegembiraan)?
- Di mana **lembah emosional** (titik rendah atau perjuangan)?

Bantu identifikasi:
- Perjuangan relatable yang menciptakan empati
- Momen mengejutkan yang menangkap perhatian
- Taruhan personal yang membuatnya penting
- Payoff memuaskan yang menciptakan resolusi

**Checkpoint output:** `emotional_arc`, `emotional_touchpoints`

---

### 3. Craft Opening Hook

Momen pertama menentukan apakah audiens terus membaca/mendengarkan.

Tanya:
- Fakta mengejutkan, pertanyaan, atau pernyataan apa yang bisa membuka cerita ini?
- Apa bagian paling menarik dari cerita ini untuk dipakai sebagai pembuka?

Arahkan menuju hook kuat yang:
- Mengejutkan atau menantang asumsi
- Mengangkat pertanyaan mendesak
- Menciptakan relatability langsung
- Menjanjikan payoff berharga
- Menggunakan detail vivid dan konkret

**Checkpoint output:** `opening_hook`

---

### 4. Write Core Narrative

Tanya mode yang diinginkan user:

1. **Draft sendiri** — user menulis, agent memberi guidance dan feedback
2. **Agent draft** — agent menulis first draft berdasarkan diskusi
3. **Co-create** — bangun cerita section by section bersama

**Jika user pilih draft sendiri:**
- Berikan writing prompts dan encouragement
- Berikan feedback pada draft yang user share
- Sarankan refinement untuk clarity, emotion, dan flow

**Jika user minta agent draft:**
- Sintesiskan semua elemen yang terkumpul
- Tulis narasi lengkap dalam tone dan gaya yang sesuai
- Strukturkan sesuai framework yang dipilih
- Sertakan detail vivid dan emotional beats
- Presentasikan draft untuk feedback dan refinement

**Jika co-create:**
- Tulis paragraf pembuka
- Dapatkan feedback dan iterate
- Bangun cerita section by section bersama

**Checkpoint output:** `complete_story`, `core_narrative`

---

### 5. Review Draft

Setelah draft selesai, tampilkan konten dan presentasikan opsi:

```
📝 Draft narasi selesai!

[Tampilkan draft lengkap]

Opsi:
  [c] Continue — lanjut ke variasi dan finalisasi
  [r] Revise — revisi bagian tertentu dari draft
  [y] YOLO — auto-complete sampai akhir
```

HALT — tunggu respons user.

---

## Output Step 2

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
  draft_mode: "agent_draft"
  status: "draft_complete"
```
