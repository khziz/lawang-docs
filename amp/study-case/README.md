---
description: Kumpulan study case AMP, dari konfigurasi connection sampai consumer.
---

# Study Case AMP

Lima kasus berikut menunjukkan cara memakai AMP dari awal sampai akhir. Tiap kasus mengikuti format yang sama.

| Bagian | Isinya |
|---|---|
| **Skenario** | Masalah yang ingin diselesaikan |
| **Komponen** | Apa saja yang dibuat |
| **Langkah** | Urutan konfigurasi |
| **Script / konfigurasi** | Isi yang dipakai |
| **Hasil** | Apa yang Anda dapatkan |

## Daftar kasus

| # | Kasus | Menggunakan |
|---|---|---|
| 1 | [Meneruskan request ke API partner](01-proxy-http.md) | Connection HTTP, rate limit, CORS |
| 2 | [Query database sebagai API](02-database-sebagai-api.md) | Connection Database, request validation |
| 3 | [CRUD items dengan JavaScript](03-crud-items-javascript.md) | Connection JavaScript, method `$GT` |
| 4 | [Menggabungkan API dan menyamarkan data](04-aggregator-dan-masking.md) | Connection JavaScript, `$GT.get`, masking |
| 5 | [Akses per departemen](05-akses-per-departemen.md) | Consumer: group, client, credential, grant service |

{% hint style="info" %}
Semua nama dan nilai (host, tabel, path, nama client) adalah **ilustrasi**. Sesuaikan dengan lingkungan Anda.
{% endhint %}

Untuk study case job terjadwal, lihat [Study Case DSP](../../dsp/study-case/README.md).
