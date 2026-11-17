---
description: "WAF, statistics server, general configs, dan blacklist IP di AMP."
---

# Pengaturan

Menu **Settings** mengatur keamanan dan perilaku seluruh server AMP.

## WAF

WAF (Web Application Firewall) di AMP:

* Berbasis **OWASP CRS** (Core Rule Set).
* Kompatibel dengan **SecLang**.
* Kompatibel dengan **ModSecurity**.
* Berisi aturan deteksi serangan yang sudah ditentukan dan teruji.

## Statistics server

Halaman ini menampilkan kondisi server secara langsung.

| Kelompok | Informasi |
|---|---|
| Status | Status server (misalnya ONLINE), alamat HTTP dan gRPC |
| Sistem | Sistem operasi, versi Go, arsitektur, jumlah worker, uptime, versi Lawang, mode prefork |
| Resource | Pemakaian CPU, beban Lawang, jumlah core CPU, pemakaian memori, memori bebas |

## General configs

### Server

| Setelan | Fungsi |
|---|---|
| **Server Configuration** | Mengatur timeout, batas request, memori, connection pool, dan worker |
| **HEAD Method** | Mengatur apakah metode HTTP `HEAD` diperbolehkan |
| **Prefork** | Membuat beberapa worker process sejak awal, sebelum menerima traffic |
| **Data Protection** | Mengatur perlindungan/enkripsi data dan log |
| **Redis Configuration** | Mengatur connection Lawang ke Redis untuk cache, session, atau data sementara |
| **Cache Configuration** | Mengatur berapa lama cache disimpan agar request berulang lebih cepat |
| **Response Compression** | Mengecilkan ukuran response agar transfer lebih efisien |
| **Case Sensitive Routes** | Menentukan apakah penulisan URL/route membedakan huruf besar dan kecil |

### Log

| Setelan | Fungsi |
|---|---|
| **System Log** | Log aktivitas utama sistem/server Lawang |
| **Activity Log** | Log aktivitas yang terjadi pada aplikasi |
| **Scheduler Log** | Log proses scheduler/job yang berjalan otomatis |
| **Log Path** | Lokasi file log disimpan |
| **Max Size** | Ukuran maksimum file log sebelum dirotasi (contoh di materi: 1024 MB) |
| **Max Backups** | Jumlah file log lama yang disimpan (contoh: 3) |
| **Max Age** | Lama file log lama dipertahankan (contoh: 7 hari) |
| **Compress Old Logs** | Mengompresi log lama untuk menghemat storage (contoh: aktif) |

### CORS

| Setelan | Fungsi |
|---|---|
| **Allowed Origins** | Domain yang boleh mengakses API |
| **Allowed HTTP Methods** | Metode HTTP yang diizinkan (contoh di materi: `POST` dan `GET`) |
| **Allowed Headers** | Header yang boleh dikirim client (contoh: `Authorization`) |
| **Exposed Headers** | Header response yang boleh dibaca browser |
| **Max Age Duration** | Lama browser menyimpan hasil pengecekan CORS (*preflight*) sebelum mengecek ulang |
| **Allow Credentials** | Mengizinkan pengiriman credential seperti cookie atau authorization |
| **Allow Private Network** | Mengizinkan atau melarang request dari jaringan internal |

### Security headers

Jika **Security Headers** aktif, AMP menambahkan header keamanan berikut.

| Header | Fungsi |
|---|---|
| **XSS Protection** | Membantu melindungi dari serangan XSS |
| **Content Security Policy (CSP)** | Mengatur sumber resource yang boleh dijalankan browser. Bisa dibiarkan belum dikonfigurasi |
| **HSTS** | Memaksa akses lewat HTTPS (punya setelan max age) |
| **X-Frame-Options: SAMEORIGIN** | Mencegah halaman di-embed oleh origin lain, membantu mencegah *clickjacking* |
| **Referrer-Policy: no-referrer** | Tidak mengirim informasi halaman sebelumnya lewat Referer |
| **Permissions-Policy: camera=()** | Menonaktifkan akses kamera lewat browser |
| **Content-Type-Options: nosniff** | Mencegah browser menebak tipe file (MIME) sembarangan |
| **Cross-Origin-Embedder-Policy: require-corp** | Membatasi resource origin lain yang boleh di-embed |
| **Cross-Origin-Opener-Policy: same-origin** | Mengisolasi *browsing context* dari origin lain |
| **Cross-Origin-Resource-Policy: same-origin** | Membatasi resource agar hanya dipakai origin yang sama |
| **Origin-Agent-Cluster: ?1** | Mengaktifkan isolasi resource berdasarkan origin |

## Blacklist IP

Untuk memblokir IP tertentu. Ada kolom **Reason for blocking** (opsional) untuk mencatat alasan pemblokiran.
