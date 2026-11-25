---
description: Menulis script JavaScript untuk job di DSP.
---

# Script job

Script job memakai **JavaScript** dengan method `$GT` yang sama seperti di AMP, dalam **mode scheduler**.

## Data job yang tersedia

| Properti       | Isi         |
| -------------- | ----------- |
| `$GT.run_id`   | ID eksekusi |
| `$GT.job_code` | Kode job    |
| `$GT.job_name` | Nama job    |

## Method khusus scheduler

| Method                           | Kegunaan                                       |
| -------------------------------- | ---------------------------------------------- |
| `$GT.run(kode)`                  | Menjalankan satu job lain dan menunggu selesai |
| `$GT.parallel([kode, ...])`      | Menjalankan beberapa job bersamaan             |
| `$GT.sync(sumber, tujuan, opsi)` | Sinkronisasi database ke database              |
| `$GT.rfc_sync(...)`              | Sinkronisasi tabel SAP ke database             |

Semua method lain (`query`, `get`, `post`, `save`, `publish`, dan seterusnya) juga bisa dipakai.

## Contoh 1: Sinkronisasi sederhana

```js
$GT.log({run: $GT.run_id, step: 'start'});

$GT.sync({
  connection: "DB-SOURCE",
  query: "SELECT * FROM table-source"
}, {
  connection: "DB-TARGET",
  table: "table-target",
  identityFields: ["id"]
}, {"worker": 4, "batchSize": 1000});

$GT.log({run: $GT.run_id, step: 'done'});
```

## Contoh 2: Beberapa job berurutan dan paralel

```js
$GT.log({run: $GT.run_id, step: 'start'});
$GT.parallel(['import-a', 'import-b']);          // dua job bersamaan
const error = $GT.run('build-summary', false);   // lalu satu job lagi
if (error !== null) { throw error; }
$GT.log({run: $GT.run_id, step: 'done'});
```

## Belajar lebih lanjut

| Anda ingin...                | Buka                                                                                         |
| ---------------------------- | -------------------------------------------------------------------------------------------- |
| Panduan job dan sinkronisasi | [Mengatur job di scheduler](../amp/connection/javascript/buat-endpoint-api/job-scheduler.md) |
| Aturan dasar script          | [Aturan dasar script](../amp/connection/javascript/methods/aturan-dasar.md)                  |
| Error dan timeout            | [Error dan timeout](../amp/connection/javascript/methods/error-dan-timeout.md)               |
| Detail semua method          | [Ringkasan semua method](../amp/connection/javascript/ringkasan-method/)                     |

## Study case

Contoh job lengkap ada di [Study Case DSP](study-case/).
