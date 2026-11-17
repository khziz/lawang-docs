# Apa itu Lawang?

**Lawang** adalah rangkaian platform dari Moklaar untuk mengelola API dan data. Lawang terdiri dari dua platform.

## AMP: API Gateway Management System

AMP adalah **pintu masuk tunggal** bagi aplikasi (web atau mobile) yang ingin mengakses banyak layanan backend. Semua request lewat AMP, sehingga keamanan, pembatasan, pencatatan log, dan konfigurasi bisa diatur di satu tempat.

Anda memakai AMP untuk:

* Mendaftarkan layanan backend (HTTP, database, JavaScript, dan lainnya).
* Menentukan endpoint dan path yang dilihat konsumen.
* Mengatur siapa yang boleh memanggil API (consumer).
* Memasang filter, rate limit, CORS, dan WAF.
* Menulis **skrip JavaScript** untuk membuat endpoint sendiri.

## DSP: Data Synchronization Platform

DSP menjalankan **job terjadwal** untuk menyinkronkan data. Misalnya menyalin data dari database kantor cabang ke database pusat setiap malam.

Anda memakai DSP untuk:

* Sinkronisasi database ke database (**Direct**).
* Sinkronisasi dari agent ke pusat (**Upsync**) dan dari pusat ke agent (**Downsync**).
* Menjadwalkan pekerjaan dengan cron.
* Menyusun alur job dengan skrip JavaScript.

## Hubungan keduanya

```text
              Skrip JavaScript ($GT)
               /               \
        AMP (Gateway)       DSP (Scheduler)
   endpoint JavaScript        script pada job
   menjawab request HTTP     sinkronisasi + jadwal
```

| | AMP | DSP |
|---|---|---|
| Dipicu oleh | Request yang masuk | Jadwal atau perintah job |
| Mode skrip | Gateway | Scheduler |
| Fungsi khusus skrip | `$GT.response` | `$GT.run`, `parallel`, `sync`, `rfc_sync` |

Fungsi JavaScript lain (`query`, `get`, `post`, `encrypt`, dan sebagainya) bisa dipakai di keduanya. Daftar lengkapnya ada di [Koneksi JavaScript](../amp/koneksi-javascript.md).

## Selanjutnya

* [AMP: Pengantar](../amp/README.md)
* [DSP: Pengantar](../dsp/README.md)
