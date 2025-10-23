## Deteksi alamat website
1. https://rsmoewardi.com/
2. https://rsudmoewardi.go.id/
3. https://old.rsmoewardi.com/frontend/home

## ANALISA DAN REKOMENDASI

# 1. Temuan cepat (fakta penting)
* RSUD Dr. Moewardi memiliki situs resmi yang memuat profil, berita, layanan, serta info kontak. ([RSUD Dr. Moewardi][1])
* Ada varian domain/portal pemerintahan (rsudmoewardi.go.id) yang juga menayangkan konten layanan publik — ini menunjukkan ada beberapa entry point/instansi domain. ([RSUD Dr. Moewardi][2])
* Fitur pendaftaran/layanan online sudah ada (sebut: pendaftaran/pelayanan rawat jalan / pendaftaran vaksin disebut di akun & halaman pendaftaran). ([RSUD Dr. Moewardi][3])
* RS memiliki kegiatan editorial (berita, jurnal) dan aktif di media sosial sehingga kebutuhan integrasi konten & SEO penting. ([RSUD Dr. Moewardi][4])

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

**Opsi A — WordPress (direkomendasikan untuk editorial-heavy, cepat di-deploy)**
* Kenapa: mudah dipakai tim non-teknis, banyak plugin untuk SEO, form, calendar, RBAC.
* Setup:
  * Single canonical domain + WordPress multisite *hanya* kalau perlu banyak sub-site; kalau tidak, satu WP instance cukup.
  * Gunakan custom post types untuk Dokter/Jadwal, Layanan, Berita.
  * Plugin: Advanced Custom Fields (ACF) / Metabox untuk struktur data; WP REST API untuk integrasi; WP Rocket / caching; security plugin (Wordfence/limit login); backup otomatis (Updraft/managed host).
  * Hosting: Managed VPS / Cloud (DigitalOcean / AWS Lightsail / cloud provider pemerintah) + CDN (Cloudflare) + SSL.
* Integrasi:
  * Integrasi ke sistem pendaftaran internal via REST API. Untuk antrian, gunakan webhook / queue.
  * Single Sign-On internal jika ada portal karyawan.

**Opsi B — Laravel (direkomendasikan bila butuh integrasi sistem & kontrol penuh)**
* Kenapa: lebih cocok jika akan mengintegrasikan EMR / HIMS / modul rekam medis, modul antrean yang kompleks, dan tim dev sudah mahir Laravel.
* Setup:
  * Backend Laravel + admin panel (Nova / Filament / Voyager) untuk CMS; API-first approach (Laravel Sanctum / Passport).
  * Frontend: Blade atau headless with Next.js / Nuxt.js untuk performance & PWA.
  * DB: MySQL / MariaDB; cache Redis; queue worker untuk background jobs (email, SMS).
  * Hosting: Cloud (AWS/GCP/OCI) dengan autoscaling dan Load Balancer.
* Keuntungan: arsitektur lebih mudah diintegrasikan ke sistem rumah sakit eksisting serta audit / log yang kuat.

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
* Desain ulang hero + 3 CTA utama; perbaiki mobile layout.
* Struktur konten: buat template halaman layanan dan dokter.
* Implementasi form booking sederhana (validasi nomor telepon, auto-confirmation via email/SMS).

### Prioritas 3 — Backend & workflow (1–3 bulan)
* Pilih CMS/stack (WP or Laravel) dan migrasi/struktur CPT (Berita, Layanan, Dokter/Jadwal).
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


## 1. Halaman Homepage ([https://rsmoewardi.com/](https://rsmoewardi.com/))

### A. Kekuatan

* Homepage langsung menampilkan beberapa layanan utama (Deteksi Mandiri Cepat COVID-19, Pendaftaran Rawat Jalan, etc). ([RSUD Dr. Moewardi][1])
* Terdapat bagian berita terkini yang menunjukkan update aktivitas rumah sakit sehingga menunjukkan dinamika dan relevansi. ([rsudmoewardi.com][2])
* Informasi kontak (alamat, telepon) terlihat di beberapa bagian (meskipun mungkin di footer atau bagian bawah) yang memudahkan akses. ([rsudmoewardi.com][2])

### B. Kelemahan / Temuan Perbaikan

1. **Performance / Loading**

   * Karena banyak elemen (banner, slideshow, image besar) di hero section — bisa memperlambat loading, terutama di mobile/area dengan koneksi lambat.
   * Tidak ada indikasi optimasi gambar (WebP, lazy load) terlihat dari sekilas; bisa jadi banyak asset berat.
2. **UI / UX – Hierarki & CTA**

   * Hero section menampilkan banyak link “Pendaftaran”, “Jadwal Dokter”, “Kamar Tersedia”— namun tombol / CTA belum tentu cukup menonjol secara visual agar pengguna langsung tahu “klik sini untuk daftar”.
   * Navigasi mungkin kurang spesifik — banyak link di hero tanpa struktur kategori jelas (“Layanan Utama”, “Informasi Pasien”, “Jadwal Dokter”) sehingga pengguna bisa bingung.
3. **Mobile Experience**

   * Tidak secara langsung saya uji responsif secara mendalam, tapi kemungkinan ada elemen yang tidak optimal di mobile (misalnya banyak kolom/slider vs. single-column).
4. **SEO / Metadata / Schema**

   * Tidak ada indikasi bahwa halaman memiliki markup schema khusus untuk rumah sakit / medical organization. Ini bisa menghambat visibilitas pencarian spesialis maupun layanan medis.
5. **Accessibility**

   * Kontras warna, ukuran teks di hero & widget berita mungkin tidak optimal untuk aksesibilitas (mis. pasien lanjut usia, mata kurang tajam).
6. **Domain / Konsolidasi**

   * Terdapat beberapa portal/sub-domain (old.rsmoewardi.com, lab.rsmoewardi.com, dll) yang bisa membingungkan pengguna serta berdampak SEO jika tidak dikelola dengan redirect yang baik. Misalnya “old.rsmoewardi.com” masih aktif. ([Old RSMoewardi][3])

### C. Rekomendasi Perbaikan

* Optimasi performa: kompres gambar hero, gunakan format modern (WebP/AVIF), implementasikan lazy-load untuk gambar dan bagian di bawah fold.
* Fokus pada 2-3 CTA utama di hero area: “Daftar Online”, “Jadwal Dokter Hari Ini”, “Hotline IGD” — dengan tombol besar dan kontras warna.
* Rearrange navigasi agar terdiferensiasi: misalnya “Layanan Medis”, “Pendaftaran & Biaya”, “Info Pasien”, “Berita & Edukasi”.
* Pastikan halaman mobile terlihat clean: single-column layout, font minimal ukuran 16px di mobile, tombol mudah diklik jari.
* Tambahkan markup schema seperti `MedicalOrganization` atau `Hospital` untuk rumah sakit, serta `Service` untuk layanan yang disediakan.
* Supaya SEO lebih kuat: meta title & description yang unik, canonical tag, sitemap.xml dan robots.txt, redirect sub-domain lama ke domain utama.
* Uji aksesibilitas (WCAG AA): periksa kontras warna, alt text gambar, navigasi keyboard, ukuran tombol & input mudah diakses.
* Konsolidasi domain/sub-domain: jika ada portal lama, lakukan redirect 301 ke domain utama agar tidak berdilusi.

---

## 2. Halaman Layanan – Contoh: FAQ ([https://rsmoewardi.com/frequently-asked-questions-faq/](https://rsmoewardi.com/frequently-asked-questions-faq/))

![Image](https://rsmoewardi.com/wp-content/uploads/2022/08/SURVEY-KEPUASAN-APLIKASI-E-PATIENT-1024x491.jpg)

![Image](https://rsmoewardi.com/wp-content/uploads/2021/06/1-1024x1024.jpg)

![Image](https://rsmoewardi.com/wp-content/uploads/brizy/imgs/WhatsApp-Image-2025-09-18-at-13.04.20-328x464x0x16x328x432x1758175479.jpeg)

![Image](https://rsmoewardi.com/wp-content/uploads/2021/06/2-1024x1024.jpg)

![Image](https://rsmoewardi.com/wp-content/uploads/sb-instagram-feed-images/348191886_632380558928526_1171660986205027727_n.webpfull.jpg)

![Image](https://rsmoewardi.com/wp-content/uploads/2024/04/WhatsApp-Image-2024-03-28-at-1.26.44-PM.jpeg)

### A. Kekuatan

* Halaman FAQ memberikan jawaban untuk pertanyaan yang umum (jadwal dokter, layanan BPJS, pendaftaran online) yang sangat bermanfaat untuk pasien. ([RSUD Dr. Moewardi][4])
* Struktur list-pertanyaan memudahkan pengguna mencari informasi — cukup ringkas dan langsung ke poin.
* Tautan internal ke alur pendaftaran & informasi layanan lainnya ada (misalnya “Alur Pendaftaran Pasien Baru” dalam FAQ) yang memperkuat jaringan konten. ([RSUD Dr. Moewardi][4])

### B. Kelemahan / Temuan Perbaikan

1. **Design / Visual Hierarki**

   * Dari tampilan sederhana: tidak ada pembeda visual yang kuat untuk setiap pertanyaan – sehingga bisa sulit dipindai dengan cepat.
   * Pada mobile, teks tampak padat; mungkin kurang whitespace dan tombol maupun link tidak cukup menonjol.
2. **Pendalaman Informasi / Struktur Data**

   * FAQ bagus untuk pertanyaan umum, tapi halaman layanan sebaiknya menambahkan lebih banyak “next step” (CTA) — seperti “Klik di sini untuk daftar online”, atau “Hubungi 0271-634634 ext …”
   * Kurang penggunaan ikon atau visual yang membantu pemahaman (misalnya ilustrasi ikon untuk BPJS, pendaftaran, jam layanan).
3. **SEO / Schema**

   * Halaman FAQ bisa menggunakan markup `FAQPage` schema untuk meningkatkan snippet di Google search. Tidak tampak bahwa markup itu sudah digunakan.
4. **Konten Dinamis**

   * Jika FAQ tidak sering diperbarui/dioptimasi, maka bisa stale dan kurang relevan. Misalnya jika jadwal berubah atau sistem pendaftaran berubah, pertanyaan-jawaban harus diperbarui.
5. **Navigasi & Back-Link ke homepage/layanan**

   * Halaman layanan harus memiliki link “kembali ke” atau breadcrumbs agar pengguna tidak tersesat (misalnya Home > Informasi Pasien > FAQ). Tidak pasti breadcrumbs ada.

### C. Rekomendasi Perbaikan

* Tambahkan markup schema `FAQPage` di bagian header halaman FAQ supaya Google bisa menampilkan rich snippet.
* Re-design layout FAQ menjadi accordion (expand/collapse) agar pengguna bisa cari pertanyaan tanpa scroll panjang, khususnya di mobile.
* Tambahkan CTA akhir setiap pertanyaan penting: misalnya “Klik untuk daftar online”, “Hubungi”, “Lihat alur pendaftaran”.
* Tambahkan ikon/ilustrasi untuk tiap kategori pertanyaan untuk visual cue (misalnya ikon kalender untuk “Jadwal Dokter”, ikon pendaftaran untuk “Pendaftaran Online”).
* Tambahkan breadcrumbs dan link ke home/layanan terkait untuk meningkatkan navigasi konteks.
* Pastikan konten FAQ di-review berkala (misalnya tiap 3 bulan) agar sesuai dengan perubahan operasional rumah sakit.
* Optimasi mobile: font minimal 16 px, spacing antar pertanyaan cukup, tombol mudah dijangkau ibu jari.

---

## 3. Ringkas Tabel Audit & Prioritas

| Halaman       | Isu Utama                                  | Prioritas       |
| ------------- | ------------------------------------------ | --------------- |
| Homepage      | Performansi lambat, CTA kurang menonjol    | **Tinggi**      |
| Homepage      | Navigasi & struktur agak kompleks          | Menengah        |
| Homepage      | SEO/schema & domain konsolidasi            | Menengah-Tinggi |
| Layanan (FAQ) | Layout dan visual hierarki kurang optimal  | Menengah        |
| Layanan (FAQ) | Schema markup & CTA internal belum lengkap | Menengah        |
| Layanan (FAQ) | Konten perlu review berkala                | Rendah          |
