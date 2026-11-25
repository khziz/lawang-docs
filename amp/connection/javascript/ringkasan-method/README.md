# Ringkasan semua method

> Mencari panduan langkah demi langkah? Lihat bagian **Panduan per tugas** di sidebar.

Tabel cepat untuk mencari method. Klik nama halaman untuk detailnya.

## Mode ketersediaan

* **G+S** = gateway dan scheduler
* **G** = gateway saja
* **S** = scheduler saja

## Utilitas & HTTP

| Method | Mode | Kegunaan | Halaman |
|---|---|---|---|
| `$GT.log(value)` | G+S | Menulis log terstruktur | [Utilitas](utilitas-logging.md) |
| `$GT.uuid()` | G+S | Membuat UUID v4 | [Utilitas](utilitas-logging.md) |
| `$GT.request(method, url, ...)` | G+S | Request HTTP dengan method bebas | [HTTP](http.md) |
| `$GT.get / delete` | G+S | Request HTTP tanpa body | [HTTP](http.md) |
| `$GT.post / put / patch` | G+S | Request HTTP dengan body | [HTTP](http.md) |
| `$GT.response(body, ...)` | G | Menentukan respons endpoint | [HTTP](http.md) |

## Database

| Method | Kegunaan |
|---|---|
| `$GT.query` | Menjalankan SELECT, hasil array object |
| `$GT.exec` | Menjalankan SQL tanpa hasil (INSERT/UPDATE/DELETE) |
| `$GT.exists` | Memeriksa apakah data ada |
| `$GT.save` | Menyimpan satu atau banyak record |
| `$GT.update` | Memperbarui satu record |
| `$GT.upsert` | Insert atau update banyak record |

Semua berada di [Database](database.md), mode G+S.

## File

| Method | Kegunaan |
|---|---|
| `$GT.load_file` | Membaca isi file (string) |
| `$GT.store_file` | Menyimpan file dari Base64 |
| `$GT.check_file` | Mengambil ukuran file (byte) |
| `$GT.mkdir` | Membuat direktori |
| `$GT.read_csv` | Membaca CSV menjadi array 2 dimensi |
| `$GT.read_excel` | Membaca sheet Excel menjadi array 2 dimensi |

Semua berada di [File](file.md), mode G+S.

## Keamanan

| Method | Kegunaan |
|---|---|
| `$GT.md5`, `$GT.sha256` | Hash hex |
| `$GT.hmac256`, `$GT.hmac512` | HMAC |
| `$GT.encrypt`, `$GT.decrypt` | Enkripsi dengan secret konfigurasi pusat |
| `$GT.sha256_signing`, `$GT.sha256_verify` | Tanda tangan RSA SHA-256 |
| `$GT.jwtrs256_signing`, `$GT.jwtrs256_verify` | JWT RS256 |

Semua berada di [Keamanan](keamanan.md), mode G+S.

## Transformasi, messaging, scheduler

| Method | Mode | Kegunaan | Halaman |
|---|---|---|---|
| `$GT.mask_prefix / mask_suffix / mask` | G+S | Menyamarkan sebagian string | [Masking](masking-transformasi.md) |
| `$GT.xml2json` | G+S | Mengubah XML jadi object | [Masking](masking-transformasi.md) |
| `$GT.transform` | G+S | Memetakan ulang struktur JSON | [Masking](masking-transformasi.md) |
| `$GT.publish` | G+S | Mengirim pesan ke event backend | [Messaging & SAP](messaging-sap.md) |
| `$GT.rfc` | G+S | Memanggil fungsi RFC SAP | [Messaging & SAP](messaging-sap.md) |
| `$GT.run` | S | Menjalankan satu job anak | [Scheduler](scheduler.md) |
| `$GT.parallel` | S | Menjalankan beberapa job anak paralel | [Scheduler](scheduler.md) |
| `$GT.sync` | S | Sinkronisasi database ke database | [Scheduler](scheduler.md) |
| `$GT.rfc_sync` | S | Sinkronisasi tabel SAP ke database | [Scheduler](scheduler.md) |
