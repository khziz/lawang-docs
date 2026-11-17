# Menulis script job

Script job memakai **JavaScript** dengan kotak peralatan `$GT` yang sama seperti di AMP, dalam **mode scheduler**.

## Data job yang tersedia

| Properti       | Isi         |
| -------------- | ----------- |
| `$GT.run_id`   | ID eksekusi |
| `$GT.job_code` | Kode job    |
| `$GT.job_name` | Nama job    |

## Fungsi khusus scheduler

| Fungsi                           | Kegunaan                                       |
| -------------------------------- | ---------------------------------------------- |
| `$GT.run(kode)`                  | Menjalankan satu job lain dan menunggu selesai |
| `$GT.parallel([kode, ...])`      | Menjalankan beberapa job bersamaan             |
| `$GT.sync(sumber, tujuan, opsi)` | Sinkronisasi database ke database              |
| `$GT.rfc_sync(...)`              | Sinkronisasi tabel SAP ke database             |

Semua fungsi lain (`query`, `get`, `post`, `save`, `publish`, dan seterusnya) juga bisa dipakai.

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

| Anda ingin...                | Buka                                                               |
| ---------------------------- | ------------------------------------------------------------------ |
| Panduan job dan sinkronisasi | [Mengatur job di scheduler](../panduan-per-tugas/job-scheduler.md) |
| Aturan dasar skrip           | [Aturan dasar skrip](../konsep-dasar/aturan-dasar.md)              |
| Error dan timeout            | [Error dan timeout](../konsep-dasar/error-dan-timeout.md)          |
| Detail semua fungsi          | [Ringkasan semua fungsi](../referensi-fungsi/ringkasan-fungsi.md)  |
