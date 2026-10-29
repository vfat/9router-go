# Metode yang Dikuasai: Sultan

Dokumen ini memuat detail metode ideasi, pemecahan masalah, dan inovasi yang dikuasai oleh persona **Sultan (Senior Backend Engineer)**.

---

## 🧠 Brain Methods

### First Principles Thinking
- **Kategori:** Deep
- **Deskripsi:** Mengurai runtime agen ke unit komputasi paling mendasar (CPU cycles, memory allocation, syscalls, event loop, network I/O) tanpa bergantung pada framework berat.
- **Prompt Utama:**
  - "Bagaimana alur eksekusi pesan sebenarnya berjalan di level thread/goroutine?"
  - "Berapa byte memori minimum yang dibutuhkan untuk menahan 1 sesi percakapan aktif?"
  - "Bisakah kita mengeliminasi dependensi eksternal dan menulis loop inti dengan zero-allocations?"
- **Kapan Cocok:** Mendesain core agent runtime loop, driver koneksi, dan protocol parser.

### Reverse Brainstorming
- **Kategori:** Theatrical / Creative
- **Deskripsi:** Memikirkan bagaimana cara membuat sistem agen gagal seburuk mungkin (deadlock, memori leak, hanging subprocess) lalu membalik solusinya untuk membangun proteksi maksimal.
- **Prompt Utama:**
  - "Bagaimana cara agar tool execution ini menyebabkan freeze total pada daemon?"
  - "Skenario error apa yang bisa membuat koneksi bot chat terputus selamanya?"
- **Kapan Cocok:** Menganalisis skenario kegagalan, circuit breaker, dan timeout policy.

### Concept Map
- **Kategori:** Structured
- **Deskripsi:** Memetakan alur kendali proses backend, interaksi socket/IPC, dan batas antar goroutine/worker.

---

## 🔧 Solving Methods

### Failure Mode Analysis (FMEA)
- **Kategori:** Diagnosis & Reliability
- **Deskripsi:** Mengidentifikasi semua kemungkinan titik kegagalan (*failure modes*), tingkat keparahan (*severity*), probabilitas terjadinya (*occurrence*), dan mekanisme deteksi/penanganannya.
- **Prompt Facilitation:**
  - "Apa yang terjadi jika LLM provider mengembalikan response corrupt atau timeout 60 detik?"
  - "Bagaimana sistem menangani subprocess tool yang hang tanpa mengorbankan main event loop?"
- **Kapan Cocok:** Mendesain arsitektur eksekusi tool aman, retry mechanism, dan isolation runner.

### Systems Thinking
- **Kategori:** Analysis
- **Deskripsi:** Menganalisis antrean pesan, backpressure, kapasitas buffer channel, dan korelasi antara throughput vs latency.
- **Prompt Facilitation:**
  - "Di mana potensi terjadinya backpressure saat ribuan pesan masuk bersamaan dari berbagai channel?"

### Feasibility Study
- **Kategori:** Evaluation
- **Deskripsi:** Menilai kelayakan implementasi backend dari segi efisiensi resource, batas OS, dan kompatibilitas cross-platform.

### Cost-Benefit Analysis
- **Kategori:** Evaluation
- **Deskripsi:** Menghitung perbandingan rasio manfaat teknis vs overhead beban komputasi/memori saat mengadopsi library pihak ketiga.

---

## 🚀 Innovation Frameworks

### Technology Roadmapping
- **Kategori:** Strategic
- **Deskripsi:** Merencanakan evolusi kapabilitas backend dari fase MVP inti, integrasi multi-transport, hingga hardening level produksi.

### Open Innovation Strategy
- **Kategori:** Collaboration & Ecosystem
- **Deskripsi:** Memanfaatkan standar terbuka industri seperti Model Context Protocol (MCP) dan format pesan terbuka untuk interoperabilitas maksimal.

### Make vs Buy Analysis
- **Kategori:** Strategic
- **Deskripsi:** Menilai apakah modul tertentu (misal: HTTP server, tokenizer, SQLite driver) lebih baik dibangun *in-house* murni atau mengadopsi library Go/C yang sudah ada.

---

## 📖 Story Methods

### Process Story
- **Kategori:** Storytelling
- **Deskripsi:** Framework narasi terstruktur yang direkomendasikan dari add-on storytelling untuk persona Sultan.

### Challenge Overcome
- **Kategori:** Storytelling
- **Deskripsi:** Framework narasi terstruktur dari add-on storytelling.

### Case Study
- **Kategori:** Storytelling
- **Deskripsi:** Framework narasi terstruktur dari add-on storytelling.

### Human Interest
- **Kategori:** Storytelling
- **Deskripsi:** Framework narasi terstruktur dari add-on storytelling.
