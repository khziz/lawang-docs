---
description: "Overview DSP (Data Synchronization Platform), scheduler job, dan sinkronisasi data."
---

# DSP: Data Synchronization Platform

DSP adalah platform Lawang untuk **menyinkronkan data** dan **menjalankan job terjadwal**.

## Peta isi

| Bagian | Isinya |
|---|---|
| [Scheduler job](scheduler-job.md) | Apa itu scheduler job dan tiga komponennya |
| [Sinkronisasi data](sinkronisasi.md) | Direct, Upsync, Downsync, serta insert dan upsert |
| [Instalasi & system requirements](instalasi.md) | Spesifikasi, prasyarat, setup awal |
| [Membuat server, schedule, dan job](membuat-job.md) | Langkah di Console |
| [Script job](script-job.md) | Menulis script JavaScript untuk job |
| [Study Case DSP](study-case/README.md) | Contoh kasus dari awal sampai akhir |

## Urutan kerja yang umum

1. Pasang Lawang dan siapkan lingkungan ([instalasi](instalasi.md)).
2. Buat **server** yang ditandai sebagai scheduler.
3. Buat **schedule** (kapan dijalankan).
4. Buat **job** (apa yang dijalankan), lalu isi script-nya.
5. Pantau hasilnya lewat **Scheduler Log** (lihat [Settings AMP](../amp/settings.md#log)).

## Hubungan dengan AMP

Script job memakai method `$GT` yang sama dengan [Connection JavaScript di AMP](../amp/connection/javascript/README.md), dalam **mode scheduler**. Method khusus scheduler: `$GT.run`, `$GT.parallel`, `$GT.sync`, `$GT.rfc_sync`.
