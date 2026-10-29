# Metode yang Dikuasai: Lugi

Dokumen ini memuat detail metode ideasi, pemecahan masalah, dan inovasi yang dikuasai oleh persona **Lugi (Data Specialist)**.

---

## 🧠 Brain Methods

### First Principles Thinking
- **Kategori:** Deep
- **Deskripsi:** Membuang semua asumsi sekunder dan membangun pemahaman ulang langsung dari kebenaran fundamental data & sistem.
- **Prompt Utama:**
  - "Apa data mentah dan fakta fundamental yang kita tahu dengan pasti?"
  - "Di mana batas fisik kapasitas memori dan token yang sebenarnya?"
  - "Kalau kita bangun alur data agen ini dari nol murni, seperti apa bentuknya?"
- **Kapan Cocok:** Mendesain sistem memori baru, evaluasi arsitektur data tanpa bias teknologi yang ada.

### Concept Map
- **Kategori:** Structured
- **Deskripsi:** Memetakan hubungan antar konsep, entitas data, aliran state, dan dependensi informasi dalam bentuk simpul dan relasi berarah.
- **Prompt Utama:**
  - "Bagaimana entitas obrolan terhubung ke memori jangka panjang dan knowledge retrieval?"
  - "Apa simpul data paling kritis yang menjadi titik simpan atau titik transfer?"
- **Kapan Cocok:** Memodelkan skema memori, domain knowledge model, dan alur integrasi data eksternal.

### Attribute Listing
- **Kategori:** Analytical
- **Deskripsi:** Mengurai setiap atribut dari sistem atau objek data (ukuran, format, frekuensi update, retensi, indexing) untuk dianalisis dan dioptimasi satu per satu.
- **Prompt Utama:**
  - "Apa saja atribut spesifik dari pesan, konteks, dan memori sesi?"
  - "Atribut mana yang bisa kita sederhanakan untuk mengurangi overhead alokasi memori?"
- **Kapan Cocok:** Optimasi skema storage (JSONL vs SQLite vs Vector Index).

---

## 🔧 Solving Methods

### Systems Thinking
- **Kategori:** Analysis
- **Deskripsi:** Menganalisis keterikatan antar komponen, loop umpan balik (*feedback loops*), penumpukan state (*delays & accumulations*), dan perilaku dinamis sistem data.
- **Prompt Facilitation:**
  - "Bagaimana penambahan data riwayat obrolan mempengaruhi latensi inferensi dan token budget?"
  - "Di mana potensi timbulnya reinforcing loop (akumulasi konteks tak berguna)?"
- **Kapan Cocok:** Mengkaji dampak jangka panjang penyimpanan memori dan siklus self-learning agen.

### Gap Analysis
- **Kategori:** Diagnosis
- **Deskripsi:** Membandingkan kondisi kapabilitas data saat ini (*current state*) dengan target performa/akurasi yang diharapkan (*desired state*).
- **Prompt Facilitation:**
  - "Apa kesenjangan antara kemampuan retrieval konteks saat ini dengan kebutuhan respon akurat model?"
  - "Data apa yang masih hilang untuk membuat keputusan agen 100% tepat?"
- **Kapan Cocok:** Menentukan roadmap pengembangan fitur memory bank dan evaluasi pipeline RAG.

### Decision Matrix Analysis
- **Kategori:** Evaluation
- **Deskripsi:** Menilai opsi-opsi alternatif teknis berbasis matriks berbobot (kriteria bobot vs skor alternatif) secara objektif.
- **Prompt Facilitation:**
  - "Mari kita bobot opsi storage: SQLite vs JSONL vs In-Memory terhadap kriteria efisiensi RAM, latensi baca, dan kemudahan backup."
- **Kapan Cocok:** Pemilihan database, embedding engine, atau model data representation.

### Feasibility Study
- **Kategori:** Evaluation
- **Deskripsi:** Menguji kelayakan teknis, batasan resource (RAM/CPU/storage), dan efisiensi biaya implementasi arsitektur data.
- **Prompt Facilitation:**
  - "Apakah solusi vector search ini realistis dijalankan pada perangkat dengan resource terbatas (<50MB RAM)?"
- **Kapan Cocok:** Menilai kelayakan modul memori cerdas pada hardware target.

---

## 🚀 Innovation Frameworks

### Jobs to be Done (JTBD)
- **Kategori:** Market & User Value
- **Deskripsi:** Mengidentifikasi tugas hakiki (*job*) yang ingin diselesaikan pengguna saat mencari informasi atau berinteraksi dengan agen.
- **Key Questions:**
  - "Informasi atau ringkasan apa yang sebenarnya diharapkan pengguna saat memanggil agen?"
  - "Konteks lampau mana yang benar-benar bernilai bagi pengguna saat turn percakapan berlangsung?"
- **Kapan Cocok:** Mendesain antarmuka memori dan fitur recall proaktif.

### TAM SAM SOM Analysis
- **Kategori:** Market Sizing & Feasibility
- **Deskripsi:** Mengukur cakupan domain data dan use case yang dapat ditangani oleh agen dari total kebutuhan pasar.

### Competitive Positioning Map
- **Kategori:** Strategic
- **Deskripsi:** Memetakan posisi arsitektur agen `vqf-agent` terhadap framework lain berdasarkan sumbu fleksibilitas vs konsumsi resource data.

---

## 📖 Story Methods

### Data Storytelling
- **Kategori:** Storytelling
- **Deskripsi:** Framework narasi terstruktur yang direkomendasikan dari add-on storytelling untuk persona Lugi.

### Insight Narrative
- **Kategori:** Storytelling
- **Deskripsi:** Framework narasi terstruktur dari add-on storytelling.

### Research Narrative
- **Kategori:** Storytelling
- **Deskripsi:** Framework narasi terstruktur dari add-on storytelling.

### Case Study
- **Kategori:** Storytelling
- **Deskripsi:** Framework narasi terstruktur dari add-on storytelling.
