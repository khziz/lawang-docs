---
description: "Pengertian scheduler job dan tiga komponennya."
---

# Scheduler job

**Scheduler job** adalah mekanisme untuk menjalankan tugas atau proses **secara otomatis** pada waktu atau interval tertentu, tanpa perlu dijalankan manual.

## Tiga komponen utama

| Komponen | Pertanyaan yang dijawab | Keterangan |
|---|---|---|
| **Trigger** | Apa yang memicu? | `manual`, `scheduler` (terjadwal), atau `child process` (dipanggil job lain) |
| **Job / task** | Apa yang dijalankan? | Script yang berisi pekerjaan |
| **Time** | Kapan dijalankan? | Jadwal cron |

```text
 TRIGGER ──► JOB ──► (selesai)
   ▲          ▲
   │          └── dijalankan di SERVER scheduler
 manual / jadwal (cron) / job lain (child process)
```

## Child process

Sebuah job dapat memanggil job lain. Di script, gunakan:

* `$GT.run('kode-job')` untuk menjalankan satu job anak.
* `$GT.parallel(['job-a', 'job-b'])` untuk menjalankan beberapa job anak bersamaan.

Lihat [Script job](script-job.md).
