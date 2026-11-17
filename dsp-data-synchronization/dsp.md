# DSP: Pengantar

DSP adalah platform Lawang untuk **menyinkronkan data** dan **menjalankan job terjadwal**.

## Isi bagian ini

| Halaman                                           | Isinya                                            |
| ------------------------------------------------- | ------------------------------------------------- |
| [Scheduler job](scheduler-job.md)                 | Apa itu scheduler job dan tiga komponennya        |
| [Fitur sinkronisasi data](sinkronisasi.md)        | Direct, Upsync, Downsync, serta insert dan upsert |
| [Instalasi dan kebutuhan sistem](instalasi.md)    | Spesifikasi, prasyarat, setup awal                |
| [Membuat server, jadwal, dan job](membuat-job.md) | Langkah di Console                                |
| [Menulis script job](script-job.md)               | Contoh skrip JavaScript untuk job                 |

## Urutan kerja yang umum

1. Pasang Lawang dan siapkan lingkungan ([instalasi](instalasi.md)).
2. Buat **server** yang ditandai sebagai scheduler.
3. Buat **schedule** (kapan dijalankan).
4. Buat **job** (apa yang dijalankan), lalu isi skripnya.
5. Pantau hasilnya lewat **Scheduler Log** (lihat [Pengaturan AMP](../amp-api-gateway/pengaturan.md)).
