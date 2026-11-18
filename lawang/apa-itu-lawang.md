---
description: Gambaran besar Lawang, AMP, dan DSP, serta hubungan di antara keduanya.
---

# Apa itu Lawang?

**Lawang** adalah rangkaian platform dari Moklaar untuk mengelola API dan data. Lawang terdiri dari dua platform.

## AMP: API Gateway Management Platform

AMP adalah **pintu masuk tunggal** bagi aplikasi (web atau mobile) yang ingin mengakses banyak layanan backend. Semua request lewat AMP, sehingga security, rate limit, logging, dan konfigurasi bisa diatur di satu tempat.

Dengan AMP Anda dapat:

* Mendaftarkan layanan backend sebagai **connection** (HTTP, database, JavaScript, dan lainnya).
* Menentukan **endpoint** dan **service** yang dilihat konsumen.
* Mengatur siapa yang boleh memanggil API lewat **consumer**.
* Memasang **filter**, rate limit, CORS, dan WAF.
* Menulis **script JavaScript** pada Connection JavaScript, dengan bantuan method `$GT`.

## DSP: Data Synchronization Platform

DSP menjalankan **job terjadwal** untuk menyinkronkan data. Misalnya menyalin data dari database cabang ke database pusat setiap malam.

Dengan DSP Anda dapat:

* Sinkronisasi database ke database (**Direct**).
* Sinkronisasi dari agent ke pusat (**Upsync**) dan dari pusat ke agent (**Downsync**).
* Menjadwalkan job dengan cron.
* Menyusun alur job dengan script JavaScript.

## Hubungan keduanya

```text
              Script JavaScript (method $GT)
               /                         \
        AMP (mode gateway)         DSP (mode scheduler)
   Connection JavaScript > endpoint     script pada job
   merespons request HTTP             sinkronisasi + jadwal
```

| | AMP | DSP |
|---|---|---|
| Dipicu oleh | Request yang masuk | Jadwal atau perintah job |
| Mode script | Gateway | Scheduler |
| Method khusus | `$GT.response` | `$GT.run`, `$GT.parallel`, `$GT.sync`, `$GT.rfc_sync` |

Method lain (`query`, `get`, `post`, `encrypt`, dan seterusnya) bisa dipakai di keduanya.

## Selanjutnya

* [AMP: Overview](../amp/README.md)
* [DSP: Overview](../dsp/README.md)
