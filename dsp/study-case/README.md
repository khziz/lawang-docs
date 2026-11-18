---
description: "Kumpulan study case DSP, dari sinkronisasi database sampai orkestrasi job."
---

# Study Case DSP

Lima kasus berikut menunjukkan cara memakai DSP dari awal sampai akhir. Tiap kasus mengikuti format yang sama.

| Bagian | Isinya |
|---|---|
| **Skenario** | Masalah yang ingin diselesaikan |
| **Komponen** | Apa saja yang dibuat |
| **Langkah** | Urutan konfigurasi |
| **Script** | Isi script job |
| **Hasil** | Apa yang Anda dapatkan |

## Daftar kasus

| # | Kasus | Menggunakan |
|---|---|---|
| 1 | [Sinkronisasi database ke database setiap malam](01-direct-sync.md) | Direct, upsert, cron |
| 2 | [Upsync: cabang ke pusat](02-upsync.md) | Agent di source |
| 3 | [Downsync: master data pusat ke cabang](03-downsync.md) | Agent di target |
| 4 | [Orkestrasi beberapa job](04-orkestrasi-job.md) | `$GT.parallel`, `$GT.run` |
| 5 | [Sinkronisasi tabel SAP ke database](05-sync-sap.md) | `$GT.rfc_sync` |

## Langkah umum setiap kasus

Setiap job butuh **server** (scheduler), **schedule**, lalu **job**. Langkahnya ada di [Membuat server, schedule, dan job](../membuat-job.md). Di study case ini, bagian itu diringkas dan fokus pada **script**.

{% hint style="info" %}
Semua nama (connection, tabel, agent, job) adalah **ilustrasi**. Sesuaikan dengan lingkungan Anda.
{% endhint %}

Untuk study case API gateway, lihat [Study Case AMP](../../amp/study-case/README.md).
