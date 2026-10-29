# Dokumentasi Fitur — 9router-go

> **Target Codebase**: `9router-go` v1.9.9 (`module 9router/proxy`)  
> **Workspace**: `/home/ubuntu/workspace/plan/9router-go`  
> **Tanggal Pembaruan**: 2026-10-29  
> **Metode**: Evidence-based Brownfield Feature Inventory (`ai-documentor`)

Dokumen ini menyajikan inventaris lengkap seluruh fitur yang diimplementasikan di dalam codebase **9router-go**, klasifikasi domain fungsionalnya, status implementasi aktual (*Active*, *Optional / Configurable*, *Admin-Gated*), serta penelusuran bukti langsung ke kode sumber Go dan router HTTP ([internal/handlers/router.go](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go)).

---

## Daftar Isi

1. [Konvensi Status & Kategori](#1-konvensi-status--kategori)
2. [Domain A: AI Gateway, Translasi Protokol & Reasoning](#2-domain-a-ai-gateway-translasi-protokol--reasoning)
3. [Domain B: Smart Routing, Combo Strategy & Model Catalog](#3-domain-b-smart-routing-combo-strategy--model-catalog)
4. [Domain C: RTK Token Saver, Output Persona & Jailbreak Presets](#4-domain-c-rtk-token-saver-output-persona--jailbreak-presets)
5. [Domain D: Koneksi Provider, Ekosistem OAuth & Proxy Pools](#5-domain-d-koneksi-provider-ekosistem-oauth--proxy-pools)
6. [Domain E: Multimodal Media, Web Tools & Batch API](#6-domain-e-multimodal-media-web-tools--batch-api)
7. [Domain F: Guardrails, Keamanan, Rate Limiting & Akses Kontrol](#7-domain-f-guardrails-keamanan-rate-limiting--akses-kontrol)
8. [Domain G: Semantic Cache, Analitik Pemakaian & Observabilitas](#8-domain-g-semantic-cache-analitik-pemakaian--observabilitas)
9. [Domain H: Auto-Config CLI Tools, Operasi Sistem, Daemon & Tray](#9-domain-h-auto-config-cli-tools-operasi-sistem-daemon--tray)
10. [Ringkasan Matriks Cakupan Fitur](#10-ringkasan-matriks-cakupan-fitur)

---

## 1. Konvensi Status & Kategori

- **Active**: Fitur terimplementasi penuh di kode sumber, terhubung ke router HTTP atau CLI, dan siap digunakan saat aplikasi berjalan.
- **Configurable**: Fitur aktif yang perilakunya dikendalikan oleh flag CLI, variabel lingkungan (`.env`), atau toggle pengaturan pada tabel `settings` di SQLite.
- **Admin-Gated**: Fitur operasional sensitif yang dilindungi oleh middleware `RequireAdminAuth` atau `RequireConsoleLogAuth` (hanya menerima sesi cookie dashboard atau token CLI lokal, menolak API key klien biasa).

---

## 2. Domain A: AI Gateway, Translasi Protokol & Reasoning

Fitur inti penerimaan request inferensi LLM dan konversi lintas protokol antara format klien dan format upstream provider.

| Nama Fitur | Status | Endpoint / Trigger Utama | Bukti Kode Sumber | Deskripsi Teknis |
|---|---|---|---|---|
| **OpenAI Chat Completions Proxy** | Active | `POST /v1/chat/completions`<br>`POST /chat/completions` | [router.go:76](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L76)<br>[internal/handlers/chat/](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/chat/) | Menerima request format standar OpenAI (streaming SSE maupun non-streaming), mendukung tool calling, vision image URL/base64, dan structured output. |
| **Anthropic Claude Messages Proxy** | Active | `POST /v1/messages`<br>`POST /messages`<br>`POST /messages/count_tokens` | [router.go:77-78](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L77-L78)<br>[internal/translator/](file:///home/ubuntu/workspace/plan/9router-go/internal/translator/) | Menangani spesifikasi Anthropic Messages API beserta penghitungan token (`count_tokens`), blok `thinking`, dan cache control headers. |
| **OpenAI Responses API & Compact** | Active | `POST /v1/responses`<br>`POST /responses`<br>`POST /responses/compact` | [router.go:86-87](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L86-L87)<br>[responses_json.go](file:///home/ubuntu/workspace/plan/9router-go/internal/translator/responses_json.go) | Mendukung endpoint `/v1/responses` generasi baru beserta pemadatan riwayat item (`compact`), terintegrasi penuh dengan resolusi combo dan pencatatan kuota. |
| **Ollama Chat API Compatibility** | Active | `POST /api/chat` | [router.go:79](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L79) | Memungkinkan klien ekosistem Ollama berbicara langsung ke 9router-go dan diteruskan ke provider cloud maupun lokal. |
| **Cross-Protocol Stream Translator** | Active | Internal Pipeline | [internal/translator/request.go](file:///home/ubuntu/workspace/plan/9router-go/internal/translator/request.go)<br>[internal/translator/gemini.go](file:///home/ubuntu/workspace/plan/9router-go/internal/translator/gemini.go) | Menerjemahkan struktur pesan, definisi skema tool, dan potongan frame SSE secara real-time antara OpenAI, Anthropic Claude, Google Gemini, dan Antigravity. |
| **Thinking & Reasoning Effort Normalization** | Active | Payload `reasoning_effort` / `thinking` | [thinking_levels.go](file:///home/ubuntu/workspace/plan/9router-go/internal/providers/thinking_levels.go)<br>[thinking_format.go](file:///home/ubuntu/workspace/plan/9router-go/internal/providers/thinking_format.go) | Memetakan level reasoning (`low`, `medium`, `high`, `max`, budget token) secara otomatis sesuai kapabilitas model target (Claude 3.7/4, Gemini 2.5/3.8, OpenAI o-series/GPT-5). |
| **Client Identity & Header Adaptation** | Active | Deteksi Header Klien | [client_identity.go](file:///home/ubuntu/workspace/plan/9router-go/internal/providers/client_identity.go)<br>[cline_identity.go](file:///home/ubuntu/workspace/plan/9router-go/internal/providers/cline_identity.go)<br>[antigravity.go](file:///home/ubuntu/workspace/plan/9router-go/internal/translator/antigravity.go) | Menyesuaikan identitas client, beta headers, dan quirk spesifik untuk agen seperti Claude Code, Cursor, Antigravity, Cline, OpenCode, dan Zed. |

---

## 3. Domain B: Smart Routing, Combo Strategy & Model Catalog

Fitur orkestrasi pemilihan model, rotasi akun multi-tier, dan sinkronisasi katalog kapabilitas model.

| Nama Fitur | Status | Endpoint / Trigger Utama | Bukti Kode Sumber | Deskripsi Teknis |
|---|---|---|---|---|
| **Multi-Strategy Combo Routing** | Active | `GET/POST /api/combos`<br>`PUT/DELETE /api/combos/{id}` | [router.go:271-288](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L271-L288)<br>[internal/handlers/chat/](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/chat/) | Menggabungkan beberapa model/provider ke dalam satu nama model virtual dengan strategi: `fallback`, `round-robin`, `sticky`, `capacity`, atau `fusion`. |
| **Auto-Free & Auto-Family Combos** | Active | `POST /api/combos/auto-free`<br>`POST /api/combos/auto-family`<br>`GET/POST /api/combos/presets` | [router.go:273-276](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L273-L276) | Generator otomatis untuk menyusun combo dari seluruh model gratis yang tersedia (`auto-free`) atau mengelompokkan berdasarkan keluarga model (`auto-family`). |
| **Smart Select Combo Rules** | Active | `POST /api/combos/reconcile`<br>`POST /api/combos/{id}/smart/preview`<br>`POST /api/combos/{id}/smart/load` | [router.go:284-286](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L284-L286) | Menyediakan pemilihan anggota combo secara dinamis berbasis aturan filter (*smart rules*) lengkap dengan fitur pratinjau (*dry-run preview*) dan rekonsiliasi. |
| **Account Cooldown & Force Fallback** | Configurable | `FORCE_FALLBACK_ON_ALL_UNAVAILABLE` | [.env.example:42](file:///home/ubuntu/workspace/plan/9router-go/.env.example#L42)<br>[errorclassify.go](file:///home/ubuntu/workspace/plan/9router-go/internal/providers/errorclassify.go) | Mengklasifikasikan error upstream (429 rate limit, kuota habis, auth error), menempatkan akun dalam masa cooldown, dan mengalihkan trafik ke akun sehat atau akun dengan sisa cooldown terpendek. |
| **Unified Model Catalog & Lookup** | Active | `GET /v1/models`<br>`GET /models/info`<br>`GET /models/{kind}`<br>`GET /v1/models/*` | [router.go:63-75](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L63-L75)<br>[registry_models.go](file:///home/ubuntu/workspace/plan/9router-go/internal/providers/registry_models.go) | Menyajikan daftar model gabungan dari seluruh koneksi aktif, mendukung pencarian wildcard, filter per tipe (`chat`, `embedding`, `image`, `audio`), serta metadata batas token & harga. |
| **Upstream Catalog Sync** | Active | `GET/POST /api/models/catalog-sync`<br>`POST /api/models/sync` | [router.go:73-74](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L73-L74)<br>[catalog_sync.go](file:///home/ubuntu/workspace/plan/9router-go/internal/providers/catalog_sync.go) | Menyinkronkan daftar model dan kapabilitas terbaru dari katalog upstream secara terjadwal di latar belakang maupun manual via API. |
| **Custom Models, Aliases & Deprecations** | Active | `GET/POST/DELETE /api/models/custom`<br>`GET/PUT/DELETE /api/models/alias`<br>`GET /api/models/deprecations` | [router.go:329-353](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L329-L353)<br>[aliases.go](file:///home/ubuntu/workspace/plan/9router-go/internal/providers/aliases.go)<br>[deprecation.go](file:///home/ubuntu/workspace/plan/9router-go/internal/providers/deprecation.go) | Mengelola penambahan model kustom, pemetaan nama alias model, daftar model yang sudah usang (*deprecated*), serta penonaktifan otomatis model bermasalah (`auto-disable`). |
| **Automated Model Health Ping Loop** | Configurable | `GET/PUT /api/models/health`<br>`POST /admin/health/reset` | [router.go:347-348](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L347-L348)<br>[internal/modelhealth/loop.go](file:///home/ubuntu/workspace/plan/9router-go/internal/modelhealth/loop.go) | Loop latar belakang yang mengirimkan probe/ping periodik melalui `ChatHandler` untuk memverifikasi kesehatan model setiap provider dan menyimpan riwayat statusnya di SQLite. |

---

## 4. Domain C: RTK Token Saver, Output Persona & Jailbreak Presets

Fitur penghematan token input/output dan modifikasi instruksi sistem per model.

| Nama Fitur | Status | Endpoint / Trigger Utama | Bukti Kode Sumber | Deskripsi Teknis |
|---|---|---|---|---|
| **RTK Tool-Result Compression** | Configurable | Flag `--rtk` (`RTK_ENABLED`)<br>`GET /api/tokensaver/rtk/filters`<br>`POST /api/tokensaver/rtk/test` | [main.go:40-43](file:///home/ubuntu/workspace/plan/9router-go/cmd/9router-go/main.go#L40-L43)<br>[internal/tokensaver/rtk.go](file:///home/ubuntu/workspace/plan/9router-go/internal/tokensaver/rtk.go)<br>[internal/tokensaver/filters/](file:///home/ubuntu/workspace/plan/9router-go/internal/tokensaver/filters/) | Mengompresi output tool CLI yang boros token (`git diff`, `git log`, `grep`, `find`, `ls`, `docker logs`, `cargo/go/jest/pytest/vitest` test output, `npm/pip` install, dll.) menggunakan 48+ aturan filter spesifik komando sambil menjaga baris bernomor & angka penting. |
| **Output Style Personas (Caveman, Ponytail, ADHD)** | Configurable | Flag `--caveman`, `--ponytail`, `--adhd`<br>`POST /api/tokensaver/caveman/test` | [main.go:45-58](file:///home/ubuntu/workspace/plan/9router-go/cmd/9router-go/main.go#L45-L58)<br>[prompts.go](file:///home/ubuntu/workspace/plan/9router-go/internal/tokensaver/prompts.go)<br>[caveman.go](file:///home/ubuntu/workspace/plan/9router-go/internal/tokensaver/caveman.go) | Menyuntikkan instruksi gaya bahasa hemat token: **Caveman** (jawaban ultra-ringkas), **Ponytail** (fokus kode minimal penjelasan), dan **ADHD** (poin tindakan langsung di awal). |
| **Compression Analytics** | Active | `GET /api/analytics/compression` | [router.go:228](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L228) | Melaporkan metrik penghematan token (jumlah token sebelum vs sesudah kompresi RTK) untuk ditampilkan di dashboard. |
| **Headroom Token-Compression Sidecar Manager** | Active | `POST /api/headroom/{start,stop,restart}`<br>`GET /api/headroom/status`<br>`ANY /api/headroom/proxy/*` | [router.go:125-144](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L125-L144) | Mengelola siklus hidup proses proxy kompresi eksternal *Headroom* beserta manajemen paket ekstra dan reverse proxy langsung dari dashboard. |
| **Jailbreak & Agent Prompt Presets** | Active | `GET/PUT /api/jailbreak`<br>`GET/PUT/DELETE /api/jailbreak/presets`<br>`GET/PUT/DELETE /api/jailbreak/models`<br>`POST /api/jailbreak/{resolve,preview}` | [router.go:233-250](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L233-L250) | Manajemen preset instruksi sistem/jailbreak yang dapat diikat per model, mendukung pengurutan prioritas, daftar pengecualian (*excludes*), impor/upload preset, dan simulasi pratinjau hasil injeksi. |

---

## 5. Domain D: Koneksi Provider, Ekosistem OAuth & Proxy Pools

Fitur manajemen akun upstream, alur autentikasi OAuth multi-vendor, dan perutean jaringan keluar (*outbound proxy*).

| Nama Fitur | Status | Endpoint / Trigger Utama | Bukti Kode Sumber | Deskripsi Teknis |
|---|---|---|---|---|
| **Multi-Account Provider Connections** | Active | `GET/POST /api/connections`<br>`PUT/DELETE /api/connections/{id}`<br>`POST /api/connections/{id}/{test,reorder}` | [router.go:187-204](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L187-L204) | CRUD akun provider dengan dukungan banyak akun per provider, pengaturan urutan prioritas, pengujian koneksi langsung, dan ekspor kunci. |
| **Custom Provider Nodes & Capacity Limits** | Active | `GET/POST/PUT/DELETE /api/provider-nodes`<br>`PUT /api/provider-nodes/{id}/limits`<br>`POST /api/provider-nodes/{id}/quota/reset` | [router.go:252-262](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L252-L262) | Menambahkan endpoint OpenAI-compatible atau Anthropic-compatible kustom lengkap dengan custom headers, batas kapasitas RPM/TPM per node, dan reset kuota. |
| **Built-in Multi-Vendor OAuth & Token Import** | Active | `GET/POST /api/oauth/{provider}/...`<br>`GET /callback` | [router.go:425-472](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L425-L472)<br>[internal/handlers/oauth/](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/oauth/)<br>[internal/proxy/oauth/](file:///home/ubuntu/workspace/plan/9router-go/internal/proxy/oauth/) | Mendukung otorisasi OAuth (PKCE, AuthCode, Device Flow, Loopback Proxy) dan impor token untuk: **Antigravity, Cline, OpenAI Codex, Kiro, Freebuff, Trae, Windsurf, Zed, Cursor, Kimchi, GitLab PAT, iFlow, Xiaomi MiMo, dan Grok CLI**. |
| **Web Session & Chrome Cookie Importer** | Active | `POST /api/providers/gemini-web/{browser-login,import-chrome}`<br>`POST /api/providers/deepseek-web/{browser-login,import-chrome}` | [router.go:192-196](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L192-L196) | Mengambil sesi login browser atau mengimpor cookie Chrome lokal untuk mengaktifkan provider berbasis web seperti Gemini Web dan DeepSeek Web beserta perpanjangan sesi otomatis. |
| **Outbound Proxy Pools** | Active | `GET/POST/PUT/DELETE /api/proxy-pools`<br>`POST /api/proxy-pools/{id}/test` | [router.go:290-294](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L290-L294) | Mengelola daftar proxy HTTP/HTTPS/SOCKS keluar yang dapat diikat ke koneksi provider tertentu untuk mendistribusikan IP keluar. |
| **One-Click Serverless Relay Deploy** | Active | `POST /api/proxy-pools/{vercel,deno,cloudflare,netlify}-deploy` | [router.go:301-304](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L301-L304) | Menyebarkan fungsi relay proxy secara otomatis ke akun Vercel, Deno Deploy, Cloudflare Workers, atau Netlify milik operator langsung dari dashboard. |

---

## 6. Domain E: Multimodal Media, Web Tools & Batch API

Fitur non-chat mencakup vektor embedding, sintesis suara, gambar, video, peralatan pencarian web, dan pemrosesan batch.

| Nama Fitur | Status | Endpoint / Trigger Utama | Bukti Kode Sumber | Deskripsi Teknis |
|---|---|---|---|---|
| **Text Embeddings Proxy** | Active | `POST /v1/embeddings`<br>`POST /embeddings` | [router.go:82](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L82)<br>[internal/handlers/media/](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/media/) | Meneruskan permintaan pembuatan vektor embedding ke provider yang mendukung (OpenAI, Gemini, GitHub Copilot, Ollama, dll.). |
| **Image Generation & Editing** | Active | `POST /v1/images/generations`<br>`POST /v1/images/edits` | [router.go:88-89](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L88-L89) | Endpoint kompatibel OpenAI DALL-E/gpt-image untuk pembuatan dan pengeditan gambar. |
| **Audio TTS, STT, Music & Sound Generation** | Active | `POST /v1/audio/{speech,transcriptions,music,sound-generation}`<br>`GET /api/media-providers/tts/*/voices` | [router.go:90-101](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L90-L101) | Mendukung Text-to-Speech (TTS), Speech-to-Text (transkripsi Whisper), pembuatan musik/efek suara, serta katalog suara dari Inworld, MiniMax, Deepgram, dan ElevenLabs. |
| **Video Generation & Editing** | Active | `POST /videos/{generations,edits,extensions}`<br>`GET /videos/{id}` | [router.go:102-105](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L102-L105) | Pembuatan video generatif, pengeditan, perpanjangan durasi video, dan polling status render video. |
| **Web Search, Scrape & Fetch Tools** | Active | `POST /search`<br>`POST /scrape`<br>`POST /web/fetch`<br>`POST /systemone` | [router.go:106-109](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L106-L109) | Endpoint utilitas bagi agen AI untuk melakukan pencarian web, ekstraksi konten halaman (*scraping*), dan pengambilan halaman web. |
| **OpenAI Files & Batches API** | Active | `GET/POST/DELETE /v1/files`<br>`GET/POST /v1/batches`<br>`POST /v1/batches/{batch_id}/cancel` | [router.go:631-650](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L631-L650)<br>[internal/handlers/batches/](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/batches/) | Penyimpanan berkas JSONL dan antrean pemrosesan asinkron kompatibel dengan spesifikasi OpenAI Batch API. |

---

## 7. Domain F: Guardrails, Keamanan, Rate Limiting & Akses Kontrol

Fitur perlindungan keamanan sistem, pembatasan kuota klien, manajemen rahasia, dan autentikasi dashboard.

| Nama Fitur | Status | Endpoint / Trigger Utama | Bukti Kode Sumber | Deskripsi Teknis |
|---|---|---|---|---|
| **Prompt Injection & Outbound Guardrails** | Configurable | Flag `--no-injection-guard`<br>`GET/POST/PUT/DELETE /api/guardrails/policies`<br>`GET /api/guardrails/logs` | [router.go:323-327](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L323-L327)<br>[router.go:615-624](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L615-L624)<br>[internal/guardrails/](file:///home/ubuntu/workspace/plan/9router-go/internal/guardrails/) | Memeriksa payload masuk terhadap pola serangan prompt injection dan memfilter stream keluar berdasarkan kebijakan (*policies*) per API key, serta mencatat pelanggaran ke log audit. |
| **Client API Keys & Model Scoping** | Active | `GET/POST/PUT/DELETE /api/keys`<br>`POST /api/keys/{id}/{rotate,reset-quota}`<br>`GET/PUT /api/keys/{id}/models` | [router.go:306-314](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L306-L314)<br>[internal/keikey/](file:///home/ubuntu/workspace/plan/9router-go/internal/keikey/) | Menerbitkan API key lokal untuk klien dengan fitur toggle aktif/nonaktif, rotasi kunci, pembatasan daftar model yang diizinkan (*allowlist*), dan batas kuota biaya/token. |
| **Subscription Plans & Sliding-Window Rate Limiter** | Active | `GET/POST/PUT/DELETE /api/plans`<br>`GET /api/plans/{id}/keys` | [router.go:315-320](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L315-L320)<br>[router.go:600-614](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L600-L614) | Mengelompokkan API key ke dalam tier paket (*Plans*) dan menegakkan batas RPM (*Requests Per Minute*), TPM (*Tokens Per Minute*), serta jumlah request konkuren secara real-time. |
| **Credential Vault & Key Rotation** | Active | `GET /api/vault/status`<br>`POST /api/vault/rotate` | [router.go:322](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L322)<br>[router.go:328](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L328) | Memantau status enkripsi penyimpanan kredensial sensitif dan melakukan rotasi kunci enkripsi vault. |
| **Dashboard Session Auth & SSO (OIDC / SAML)** | Configurable | `POST /api/auth/{login,logout}`<br>`POST /api/auth/oidc/test`<br>`POST /api/auth/saml/test`<br>`GET /api/auth/saml/metadata` | [router.go:403-405](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L403-L405)<br>[router.go:552-555](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L552-L555)<br>[internal/handlers/sso/](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/sso/) | Autentikasi sesi dashboard menggunakan JWT cookie (`JWT_SECRET`) serta dukungan pengujian dan metadata Single Sign-On berbasis OpenID Connect (OIDC) dan SAML 2.0. |
| **SSRF & TLS Fallback Protection** | Configurable | `STRICT_SSL` | [.env.example:47](file:///home/ubuntu/workspace/plan/9router-go/.env.example#L47)<br>[internal/handlerutil/ssrf.go](file:///home/ubuntu/workspace/plan/9router-go/internal/handlerutil/ssrf.go) | Memblokir upaya Server-Side Request Forgery (SSRF) ke jaringan privat/loopback pada URL input pengguna, serta mengatur kebijakan verifikasi sertifikat TLS upstream. |

---

## 8. Domain G: Semantic Cache, Analitik Pemakaian & Observabilitas

Fitur caching semantik, pelacakan kuota dan biaya real-time, metrik sistem, serta inspeksi debugging.

| Nama Fitur | Status | Endpoint / Trigger Utama | Bukti Kode Sumber | Deskripsi Teknis |
|---|---|---|---|---|
| **Persistent Semantic Cache** | Active | `GET/DELETE /api/cache`<br>`GET/DELETE /api/cache/entries` | [router.go:56-58](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L56-L58)<br>[router.go:222-225](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L222-L225) | Menyimpan respons LLM di memori LRU + SQLite untuk melayani prompt yang identik/semantik serupa secara instan, dilengkapi API lihat & hapus entri cache dari dashboard. |
| **Real-Time Usage SSE Stream & Topology** | Active | `GET /api/usage/stream`<br>`GET /usage/stream` | [router.go:149-150](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L149-L150)<br>[usage_stream.go](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/usage_stream.go) | Memancarkan event Server-Sent Events (SSE) setiap kali ada request masuk/selesai untuk menggerakkan animasi topologi live di dashboard. |
| **Usage Analytics, Charts & Window Cache** | Active | `GET /api/usage/{stats,chart,dashboard}` | [router.go:151-155](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L151-L155)<br>[usage_stats.go](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/usage_stats.go)<br>[usage_chart.go](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/usage_chart.go)<br>[usage_window_cache.go](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/usage_window_cache.go) | Menghitung agregasi pemakaian token, estimasi biaya, rasio keberhasilan/error, dan grafik deret waktu dengan dukungan rentang waktu kustom (`12h`, `14d`, dll.) yang dioptimalkan oleh in-memory window cache. |
| **Request Payload Inspector** | Active | `GET /api/usage/request-details`<br>`GET /api/usage/request-details/{id}` | [router.go:156-157](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L156-L157) | Menyimpan dan menampilkan detail penuh header, body request awal, body setelah translasi, hingga respons upstream untuk keperluan audit dan debugging. |
| **Provider Quota Tracker & Codex Reset Credits** | Active | `GET /api/usage/providers`<br>`GET /api/usage/{connectionId}`<br>`GET/POST /api/usage/{connectionId}/reset-credits/*` | [router.go:158-161](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L158-L161)<br>[internal/usagetracker/](file:///home/ubuntu/workspace/plan/9router-go/internal/usagetracker/) | Mem-parsing header sisa kuota dari berbagai provider dan mengelola konsumsi kredit reset kuota khusus akun Codex. |
| **Prometheus Metrics & Debug Traces** | Active | `GET /metrics`<br>`GET /api/metrics`<br>`GET /debug/traces`<br>`GET /debug/pprof/*` | [router.go:164](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L164)<br>[router.go:321](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L321)<br>[router.go:583-590](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L583-L590)<br>[internal/observ/](file:///home/ubuntu/workspace/plan/9router-go/internal/observ/) | Eksposur metrik standar Prometheus, statistik latensi p50/p95 per provider+model (`/debug/traces`), serta Go runtime profiling (`pprof`, aktif jika `PPROF_ENABLED=true` dan terautentikasi admin). |
| **Live Translator Console Logs** | Admin-Gated | `GET/DELETE /api/translator/console-logs`<br>`GET /api/translator/console-logs/stream`<br>`GET/PUT /api/translator/console-logs/level` | [router.go:414-423](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L414-L423)<br>[consolelog.go](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/consolelog.go) | Streaming log konsol internal secara langsung ke browser lengkap dengan filter level log dinamis (`DEBUG`, `INFO`, `WARN`, `ERROR`). |

---

## 9. Domain H: Auto-Config CLI Tools, Operasi Sistem, Daemon & Tray

Fitur integrasi sekali klik ke tool coding lokal, pemeliharaan database, tunneling jaringan, proses latar belakang, dan aplikasi desktop.

| Nama Fitur | Status | Endpoint / Trigger Utama | Bukti Kode Sumber | Deskripsi Teknis |
|---|---|---|---|---|
| **One-Click CLI Coding Tools Configurator** | Active | `GET /api/cli-tools/all-statuses`<br>`GET/POST/DELETE /api/cli-tools/{id}-settings`<br>`GET/POST/DELETE /api/cli-tools/hermes-*` | [router.go:674-707](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L674-L707)<br>[internal/clientconfig/](file:///home/ubuntu/workspace/plan/9router-go/internal/clientconfig/) | Mendeteksi status instalasi dan menulis file konfigurasi lokal secara otomatis agar mengarah ke `9router-go` untuk 10 agen CLI: **Claude Code (`claude`), Codex (`codex`), OpenCode (`opencode`), Droid (`droid`), Crush (`crush`), Kilo (`kilo`), Cline (`cline`), Pi (`pi`), OMP (`omp`), dan Hermes (`hermes`)**. |
| **Embedded Svelte 5 Web Dashboard & White-Label** | Active | `GET /`, `GET /dashboard/*`<br>`POST /api/settings/logo`<br>`GET /uploads/*` | [router.go:371](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L371)<br>[router.go:498-532](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L498-L532)<br>[web/embed.go](file:///home/ubuntu/workspace/plan/9router-go/web/embed.go) | Menyajikan antarmuka manajemen penuh dari dalam binary tunggal tanpa file eksternal, serta mendukung penggantian logo kustom (*white-label*). |
| **Database Backup, Prune & Vacuum** | Active | `GET/POST /api/settings/database`<br>`GET /api/settings/database/stats`<br>`POST /api/settings/database/{prune,prune-usage,vacuum}` | [router.go:362-367](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L362-L367) | Fitur ekspor/impor file database SQLite, pemangkasan riwayat penggunaan lama (`prune`), dan kompaksi ruang penyimpanan (`VACUUM`) langsung dari halaman Settings. |
| **Tunnel & Tailscale Exposure** | Active | `GET /api/tunnel/status`<br>`POST /api/tunnel/{enable,disable}`<br>`GET/POST /api/tunnel/tailscale-*` | [router.go:395-400](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L395-L400)<br>[internal/auth/tunnel.go](file:///home/ubuntu/workspace/plan/9router-go/internal/auth/tunnel.go) | Mengekspos gateway lokal secara aman ke internet atau jaringan Tailscale dengan satu klik dari dashboard. |
| **Background Daemon & Terminal Menu** | Active | `9router-go --background`<br>`9router-go {start,stop,restart,status,logs}` | [main.go:69-73](file:///home/ubuntu/workspace/plan/9router-go/cmd/9router-go/main.go#L69-L73)<br>[daemon_cmd.go](file:///home/ubuntu/workspace/plan/9router-go/cmd/9router-go/daemon_cmd.go)<br>[menu.go](file:///home/ubuntu/workspace/plan/9router-go/cmd/9router-go/menu.go)<br>[internal/daemon/](file:///home/ubuntu/workspace/plan/9router-go/internal/daemon/) | Menjalankan proses secara terpisah dari terminal (*detached daemon*), mengelola file PID di `$DATA_DIR/run/gateway.pid`, menunggu kesiapan `/health`, atau menampilkan menu TUI interaktif saat dijalankan di terminal. |
| **Cross-Platform OS Service Autostart** | Active | `9router-go service {install,uninstall,start,stop,restart,status}` | [main.go:166-203](file:///home/ubuntu/workspace/plan/9router-go/cmd/9router-go/main.go#L166-L203)<br>[service_cmd.go](file:///home/ubuntu/workspace/plan/9router-go/cmd/9router-go/service_cmd.go)<br>[internal/service/](file:///home/ubuntu/workspace/plan/9router-go/internal/service/) | Memasang dan mengelola unit layanan autostart saat boot untuk **systemd**, **macOS launchd**, **OpenRC**, **runit**, **cron `@reboot`**, maupun Windows Scheduled Task. |
| **Self-Updater & Remote Shutdown** | Admin-Gated | `9router-go update`<br>`GET /api/version/check`<br>`POST /api/version/{update,auto-update,shutdown}` | [router.go:493-496](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L493-L496)<br>[router.go:576-578](file:///home/ubuntu/workspace/plan/9router-go/internal/handlers/router.go#L576-L578)<br>[internal/updater/](file:///home/ubuntu/workspace/plan/9router-go/internal/updater/) | Memeriksa rilis baru di GitHub, memverifikasi checksum SHA-256, mengganti binary secara atomik, dan me-restart proses secara aman. |
| **Desktop System Tray App** | Active | Binary `cmd/9router-tray` | [cmd/9router-tray/main.go](file:///home/ubuntu/workspace/plan/9router-go/cmd/9router-tray/main.go)<br>[cmd/9router-tray/gui.go](file:///home/ubuntu/workspace/plan/9router-go/cmd/9router-tray/gui.go) | Aplikasi pendamping berbasis `getlantern/systray` untuk mengontrol start/stop server dan membuka dashboard langsung dari system tray OS. |

---

## 10. Ringkasan Matriks Cakupan Fitur

| Kode Domain | Nama Domain Fitur | Jumlah Kapabilitas Utama | Status Implementasi |
|---|---|:---:|:---:|
| **Domain A** | AI Gateway, Translasi Protokol & Reasoning | 7 | 100% Active |
| **Domain B** | Smart Routing, Combo Strategy & Model Catalog | 8 | 100% Active |
| **Domain C** | RTK Token Saver, Output Persona & Jailbreak Presets | 5 | 100% Active / Configurable |
| **Domain D** | Koneksi Provider, Ekosistem OAuth & Proxy Pools | 6 | 100% Active |
| **Domain E** | Multimodal Media, Web Tools & Batch API | 6 | 100% Active |
| **Domain F** | Guardrails, Keamanan, Rate Limiting & Akses Kontrol | 6 | 100% Active / Configurable |
| **Domain G** | Semantic Cache, Analitik Pemakaian & Observabilitas | 7 | 100% Active |
| **Domain H** | Auto-Config CLI Tools, Operasi Sistem, Daemon & Tray | 8 | 100% Active |
| **Total** | **8 Domain Fungsional** | **53 Kapabilitas Inti** | **Terverifikasi Penuh** |

