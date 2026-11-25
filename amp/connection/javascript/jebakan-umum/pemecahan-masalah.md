# Troubleshooting

Cari gejala yang Anda lihat, lalu periksa penyebabnya.

## Method `$GT` tidak ditemukan / tidak jalan

* Pastikan Anda menulis `$GT.nama` secara langsung, bukan `$GT[nama]` atau destructuring.
* Cek ejaan nama method (misalnya `load_file`, bukan `loadFile`).
* Pastikan method itu ada di mode Anda. `response` hanya gateway, sedangkan `run`, `parallel`, `sync`, `rfc_sync` hanya scheduler.

## `ReferenceError: fetch is not defined` (atau `require`, `Buffer`, `setTimeout`)

Itu memang tidak tersedia. Lihat [Aturan dasar](../methods/aturan-dasar.md#3-tidak-ada-fetch-settimeout-require-dan-kawan-kawannya).

## Endpoint tidak mengembalikan response yang saya harapkan

* Pada script biasa, `$GT.response(...)` harus menjadi **ekspresi terakhir** script.
* Pada modul, `return` di dalam **default export**.
* Di gateway non-production, `console.log` dapat ikut tercetak di body respons. Pakai `$GT.log` agar respons tetap bersih.

## Hasil query kosong `[]`

* Cek nilai parameter (tipe angka vs teks).
* Cek nama `source` dan tabel.
* Ingat batas 1.000 baris.

## Error connection atau "backend tidak ditemukan"

* Nama `source` harus **persis** sama dengan yang ada di layanan pusat.
* Backend tersebut harus berjenis yang sesuai (database untuk `query`, file untuk `load_file`, event untuk `publish`, SAP untuk `rfc`).

## Selalu timeout atau waktu tunggu tidak sesuai

* Timeout `0` atau di atas `RequestTimeout` diganti nilai konfigurasi.
* Gunakan angka positif.

## Request HTTP berhasil tapi datanya lama

Request `GET` dapat memakai cache sesuai `BackendCacheLifetime`. Periksa konfigurasi tersebut bila data tampak basi.

## `$GT.publish` menolak payload

Payload harus **object atau array**, bukan string JSON. Gunakan `JSON.parse` dulu.

## `$GT.decrypt` / `$GT.sha256_verify` melempar error

* `decrypt`: pakai secret yang sama dengan `encrypt`. Teks Base64 tidak boleh rusak.
* `sha256_verify`: cek format PEM (`BEGIN PUBLIC KEY`, PKIX) dan Base64 signature.

## `$GT.mask` melempar error indeks

Nilai `end` terlalu besar. Pakai paling besar `panjang − 1`.

## `rfc_sync` berhenti tanpa membaca output

Tambahkan `parameters: {}` pada bagian `source`.

## Script gagal di tengah dan saya ingin tahu penyebabnya

1. Bungkus bagian yang dicurigai dengan `try/catch`.
2. Catat dengan `$GT.log({langkah: '...', error: String(err)})`.
3. Pecah script menjadi langkah-langkah kecil dan log setiap langkah.

Belum ketemu? Lihat daftar lengkap di [Jebakan umum](./).
