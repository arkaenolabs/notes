## Deteksi alamat website
1. https://rsmoewardi.com/
2. https://rsudmoewardi.go.id/
3. https://old.rsmoewardi.com/frontend/home

## ANALISA DAN REKOMENDASI

# 1. Temuan cepat (fakta penting)
* RSUD Dr. Moewardi memiliki situs resmi yang memuat profil, berita, layanan, serta info kontak.
* Ada varian domain/portal pemerintahan (rsudmoewardi.go.id) yang juga menayangkan konten layanan publik — ini menunjukkan ada beberapa entry point/instansi domain.
* Fitur pendaftaran/layanan online sudah ada (sebut: pendaftaran/pelayanan rawat jalan / pendaftaran vaksin disebut di akun & halaman pendaftaran).
* RS memiliki kegiatan editorial (berita, jurnal) dan aktif di media sosial sehingga kebutuhan integrasi konten & SEO penting.
* Menggunakan Framework Wordpress version 5.3.14 (lama)
* Hampir semua plugin tidak update
* Webserver Menggunakan Apache/2.4.25
* Rest APi aktif dengan beberapa endpoint sudah dilakukan hardening authentikasi

# 2. Analisa mendalam & rekomendasi

## A. Struktur website / Information Architecture (IA)
Masalah umum yang sering muncul pada portal rumah sakit pemerintahan: banyak layanan terpisah (pendaftaran, jadwal, pengumuman) tersebar di subdomain atau halaman berbeda; navigation kurang konsisten.

Rekomendasi:
1. Konsolidasi *entry points* penting:
   * Buat halaman hub (landing) utama terpadu: “Layanan Utama” (IGD, Rawat Jalan, Vaksinasi, Jadwal Dokter, Pendaftaran Online, Kontak Darurat).
   * Pastikan canonical domain jelas (pilih 1 domain utama, redirect 301 dari varian) sehingga SEO & link equity terkonsolidasi.
     
2. Struktur navigasi (top-level):
   * Home | Tentang | Layanan (dropdown per layanan klinis) | Jadwal & Dokter | Informasi Pasien (Pendaftaran, Biaya, Asuransi) | Berita & Edukasi | Hubungi & Darurat
     
3. Halaman layanan harus memakai format terstruktur: ringkasan singkat → fasilitas → alur pendaftaran → FAQ → CTA (daftar janji / telepon / chat).
4. Peta situs XML & halaman “Aksesibilitas & Layanan” yang mudah ditemukan untuk pasien lanjut usia.

## B. UI / UX
Kesan yang ingin dicapai: simple, clean, profesional, cepat diakses (pasien butuh info cepat).
Masalah potensial: teks kecil, kontras tidak konsisten, banner/slider yang berat, CTA pendaftaran kurang menonjol.

Rekomendasi desain UI/UX:
1. Visual & hierarchy:
   * Gunakan grid bersih, whitespace, dan tipografi besar untuk informasi darurat/nomor hotline.
   * Warna primer: gunakan hijau/ biru profesional + aksen kontras untuk CTA (contoh: hijau gelap + aksen oranye untuk tombol penting).
     
2. Landing — above-the-fold:
   * Tampilkan 3 CTA utama: (1) Pendaftaran Rawat Jalan / Booking, (2) Informasi IGD (hotline), (3) Cari Dokter/Jadwal.
   * Tambahkan cards cepat: Jam layanan, status IGD (sibuk/normal), info penting seperti vaksinasi.
     
3. Mobile-first & responsif:
   * Pastikan tombol besar, form pendaftaran single-column, dan input nomor/ID pasien mudah diisi.
     
4. Accessibility:
   * WCAG AA: kontras warna, keyboard navigable, teks alternatif pada gambar, ukuran font bisa disesuaikan.
     
5. Performance UX:
   * Hindari slider berat; gunakan gambar hero statis yang dioptimalkan (WebP), lazy-load untuk gambar berita.
   * Tambahkan markup schema `FAQPage` di bagian header halaman FAQ supaya Google bisa menampilkan rich snippet
   * Re-design layout FAQ menjadi accordion (expand/collapse) agar pengguna bisa cari pertanyaan tanpa scroll panjang, khususnya di mobile.
   * Tambahkan CTA akhir setiap pertanyaan penting: misalnya “Klik untuk daftar online”, “Hubungi”, “Lihat alur pendaftaran”.
   * Tambahkan ikon/ilustrasi untuk tiap kategori pertanyaan untuk visual cue (misalnya ikon kalender untuk “Jadwal Dokter”, ikon pendaftaran untuk “Pendaftaran Online”).
   * Tambahkan breadcrumbs dan link ke home/layanan terkait untuk meningkatkan navigasi konteks.
   * Pastikan konten FAQ di-review berkala (misalnya tiap 3 bulan) agar sesuai dengan perubahan operasional rumah sakit.
   * Optimasi mobile: font minimal 16 px, spacing antar pertanyaan cukup, tombol mudah dijangkau ibu jari.

## C. Pengelolaan konten (CMS / editorial workflow)
Saat ini terlihat ada berita, jurnal, pengumuman, pendaftaran; penting memisahkan tipe konten dan alur publikasi.

Rekomendasi CMS & proses:
1. Struktur konten / content types:
   * CPT: Berita/Artikel, Pengumuman (time-sensitive), Halaman Layanan, Dokter & Jadwal (master data), FAQ, Download (form), Jurnal (scholarly).
2. Workflow editorial:
   * Role: Editor, Author, Reviewer, Publisher. Gunakan staging environment dan preview sebelum publish.
   * Jadwalkan konten “announcements” dengan tanggal mulai/akhir (auto-unpublish).
3. Integrasi pendaftaran & antrian:
   * Konten yang berkaitan (jadwal dokter) harus terhubung ke sistem pendaftaran sehingga pasien bisa langsung book dari halaman dokter.
4. Metadata & SEO:
   * Gunakan meta description, open graph, JSON-LD schema untuk tiap halaman layanan dan artikel (Hospital schema, MedicalOrganization, Physician).
5. Multichannel:
   * Buat RSS / API publik untuk feed berita sehingga akun media sosial bisa auto-push.

## D. Teknologi & arsitektur yang disarankan
Pertimbangkan kebutuhan: pengelolaan konten dinamis, pendaftaran online, integrasi internal (Sistem Informasi Rumah Sakit), keamanan, ketersediaan.
Pilihan arsitektur praktis — dua opsi tergantung kapasitas tim dan kebijakan pemerintahan:

** Framework WordPress (direkomendasikan untuk editorial-heavy, cepat di-deploy)**
* Kenapa: mudah dipakai tim non-teknis, banyak plugin untuk SEO, form, calendar, RBAC.
* Setup:
  * Single canonical domain + WordPress multisite *hanya* kalau perlu banyak sub-site; kalau tidak, satu WP instance cukup.
  * Gunakan custom post types untuk Dokter/Jadwal, Layanan, Berita.
  * Plugin: Advanced Custom Fields (ACF) / Metabox untuk struktur data; WP REST API untuk integrasi; WP Rocket / caching; security plugin (Wordfence/limit login); backup otomatis (Updraft/managed host).
  * Hosting: Managed VPS / Cloud (DigitalOcean / AWS Lightsail / cloud provider pemerintah) + CDN (Cloudflare) + SSL.
* Integrasi:
  * Integrasi ke sistem pendaftaran internal via REST API. Untuk antrian, gunakan webhook / queue.
  * Single Sign-On internal jika ada portal karyawan.

**Fitur teknis yang wajib/kuat disarankan (terlepas pilihan):**
* Sistem antrian & booking real-time (slot management), notifikasi via SMS/WhatsApp/email.
* Audit & logging (HIPAA-esque posture — di Indonesia patuhi aturan data medik).
* Backup & disaster recovery teratur; image dan DB snapshot.
* Monitoring & uptime (Prometheus/Grafana atau NewRelic).
* Rate-limiting & WAF (Cloudflare / Sucuri) + vulnerability scanning.
* Otentikasi dua faktor (untuk admin).

## E. Keamanan & kepatuhan
* Enkripsi HTTPS (Let's Encrypt/managed cert).
* Proteksi data pasien: enkripsi data sensitif di DB, pembatasan akses, logging akses, minimalisasi data yang ditampilkan publik.
* Kebijakan retention & delete untuk data pendaftaran.
* Implementasi CSP, XSS/CSRF protection, input sanitization.

# 3. Roadmap & prioritas (praktis, dapat dikerjakan sekarang)
Saya bagi menjadi 4 sprint/prioritas — tiap item dapat langsung diambil tim dev / IT rumah sakit.

### Prioritas 1 — Quick wins (1–2 minggu)
* Pastikan domain canonical + redirect 301 untuk duplikat domain. (SEO & trust).
* Tambah tombol hotline/IGD di header, dan buat CTA “Daftar / Booking” jelas di top bar.
* Optimasi performa: compress images (WebP), enable caching, minify CSS/JS, pasang CDN (Cloudflare).
* Pasang analytics & event tracking (Google Analytics / GA4, Google Search Console).

### Prioritas 2 — Pengalaman pengguna & aksesibilitas (2–4 minggu)
* Desain ulang slider + 3 CTA utama; perbaiki mobile layout.
* Struktur konten: buat template halaman layanan dan dokter.
* Implementasi form booking sederhana (validasi nomor telepon, auto-confirmation via email/SMS).

### Prioritas 3 — Backend & workflow (1–3 bulan)
* Buat editorial workflow (draft → review → publish).
* Integrasikan pendaftaran online ke sistem antrean/antrian bila tersedia.

### Prioritas 4 — Integrasi & maturity (3–6 bulan)
* Integrasi penuh dengan HIMS/EMR / sistem internal (API).
* Tambah PWA untuk akses cepat via mobile (offline minimal).
* Audit keamanan & uji penetrasi, SLA hosting, DR plan.

# 4. Contoh specific technical tasks (bisa langsung dikodekan)
* Buat endpoint `/api/dokter` yang mengembalikan JSON (nama, spesialis, jadwal, booking_url) — agar aplikasi mobile/portal bisa memanggil.
* Tambahkan JSON-LD schema `MedicalOrganization` dan `Physician` pada halaman layanan/dokter.
* Template CMS: single page untuk dokter dengan CTA “Book Appointment” yang memanggil modal booking (AJAX) tanpa pindah halaman.
* Implementasikan server-side rate limit pada endpoint pendaftaran (throttle).

# 5. KPI & metrik yang harus dipantau
* Waktu muat halaman (LCP, FCP) — target LCP < 2.5s.
* Conversion rate pendaftaran online (klik CTA → form submitted).
* Bounce rate halaman info darurat.
* Uptime monitoring & error rate API.
* SEO: kenaikan impressions / clicks (GSC).

# 6. Risiko & catatan operasional
* Jika terdapat lebih dari satu domain/portal, harus ada kebijakan siapa pemilik konten & yang menjadi sumber kebenaran (single source of truth). ([RSUD Dr. Moewardi][2])
* Integrasi ke sistem internal memerlukan koordinasi IT rumah sakit dan pertimbangan keamanan data pasien (perizinan & enkripsi).
* Jika ingin migrasi besar (mis. dari portal lama ke platform baru), siapkan migration plan + rollback.

# 7. Penutup — rekomendasi ringkas dan langkah selanjutnya
Rekomendasi ringkas: konsolidasikan domain → prioritas UX mobile + CTA pendaftaran → pilih stack (WordPress cepat & murah untuk editorial; Laravel kalau butuh integrasi HIMS skala enterprise) → bangun API & integrasi antrian → implementasi keamanan & monitoring.
