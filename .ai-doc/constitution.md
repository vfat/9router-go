# Constitution for `9router-go`

## Purpose

Konstitusi ini mendefinisikan aturan dan standar dokumentasi serta analisis arsitektur berbasis bukti (*evidence-based brownfield documentation*) untuk proyek **9router-go**.

`9router-go` merupakan aplikasi reverse proxy & AI router tunggal (single binary Go + embedded Svelte SPA) dengan fitur optimasi token (RTK), auto-fallback, multi-account routing, dan translasi protokol antar-provider LLM (OpenAI, Anthropic Claude, Gemini, Ollama, dll).

Workflow dokumentasi mengacu pada pedoman standar `ai-documentor`.

---

## 1. Source of Truth

- **Codebase adalah source of truth utama**: semua klaim teknis harus traceable ke kode sumber Go, konfigurasi runtime, skema database, atau dokumentasi arsitektur internal.
- **Dilarang mengambil kesimpulan hanya dari nama file/folder**.
- Dokumen warisan atau upstream (`decolua/9router`) hanya diperlakukan sebagai referensi historis/kompatibilitas, bukan otoritas mutlak atas perilaku runtime Go.

---

## 2. Workspace & Progress Tracking

- Ruang kerja dokumentasi berada di folder `.ai-doc/`.
- File pelacak progress wajib: `.ai-doc/3p.md` (`Progress`, `Plan`, `Pending`).
- File `3p.md` harus dibaca sebelum melanjutkan pekerjaan dan diperbarui setiap menyelesaikan langkah yang bermakna.

---

## 3. Evidence Rules & Penulisan Konservatif

- Setiap penjelasan arsitektur, flow eksekusi, atau komponen wajib menyertakan path kode atau file referensi konkret.
- Jika ada hal yang belum terverifikasi secara tuntas atau bukti kode masih terbatas, gunakan label transparan:
  - `Draft`
  - `Partial`
  - `Placeholder`
  - `Asumsi`
  - `Perlu Dikonfirmasi`
- Status pengujian atau build harus jujur mencerminkan apa yang benar-benar diverifikasi di lingkungan lokal.

---

## 4. Only-On-Request Rule

Artefak turunan berikut **hanya boleh dibuat bila diminta secara eksplisit oleh user**:
- `Dokumentasi-Fitur.md`
- `Dokumentasi-Komponen-Usecase.md`
- `C4-Component-Diagrams.md`
- `desain-component-document/` (DCD)
- `desain-database-document/` (ERD / DD)
- `rest-api-doc/` (API Specs / Endpoint list)

Dokumentasi dasar fase brownfield difokuskan pada:
- `.ai-doc/constitution.md` (Control plane)
- `.ai-doc/3p.md` (Progress tracker)
- `.ai-doc/Dokumentasi-Codebase.md` (Baseline dokumentasi sistem & arsitektur codebase)

---

## 5. Klasifikasi Komponen & Objek

Mengikuti taksonomi standar arsitektur `ai-documentor`:
- **Boundary**: HTTP Ingress/Router surfaces, Middleware otentikasi/CORS, CLI commands/flags, Web UI embedded handler.
- **Control**: Engine routing & combo strategy, Provider executors, Model health loop, Format translator (OpenAI <-> Claude <-> Gemini), Token saver & RTK compression engine, Quota & rate limiter manager, Updater & daemon controller.
- **Entity**: Data models (Provider connection, API Key, Combo, Usage record, Quota, Leases), DTOs request/response, Schema konfigurasi SQLite, dan **seluruh endpoint API** (diklasifikasikan sebagai `Entity` sesuai aturan dokumentasi).
- **Infrastructure**: SQLite database connection (`modernc.org/sqlite`), Uber Fx lifecycle container, HTTP Server engine (`net/http` + Chi), Process manager / daemon backend (systemd, Windows service, background detached).

---

## 6. Bahasa

- Seluruh dokumentasi teknis disusun dalam **Bahasa Indonesia yang teknis, lugas, dan praktis**.
- Istilah teknis standar industri (misal: *reverse proxy, fallback, token compression, stream SSE, payload, round-robin*) tetap dipertahankan sesuai konvensi.
