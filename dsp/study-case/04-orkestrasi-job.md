# Kasus 4: Orkestrasi beberapa job

## Skenario

Setiap malam, dua impor (`import-a` dan `import-b`) harus selesai dulu. Setelah itu, job `build-summary` merangkum hasilnya. Kedua impor tidak saling bergantung, jadi bisa jalan bersamaan.

## Komponen

| Job | Tugas |
|---|---|
| `import-a` | Sinkronisasi data sumber A |
| `import-b` | Sinkronisasi data sumber B |
| `build-summary` | Membuat ringkasan setelah impor selesai |
| `nightly-pipeline` | **Job induk** yang mengatur urutan ketiganya |

```text
 nightly-pipeline
   ├─ parallel ─┬─ import-a
   │            └─ import-b
   └─ run ───────── build-summary   (setelah kedua import selesai)
```

## Langkah

1. Buat job `import-a`, `import-b`, dan `build-summary` dengan script masing-masing (misalnya `$GT.sync`, lihat [Kasus 1](01-direct-sync.md)).
2. Buat job induk `nightly-pipeline` dengan schedule malam hari, lalu isi script di bawah.

## Script job induk

```js
$GT.log({run: $GT.run_id, step: 'start'});

$GT.parallel(['import-a', 'import-b']);          // dua job bersamaan, tunggu keduanya

const error = $GT.run('build-summary', false);   // lalu satu job lagi
if (error !== null) {
  $GT.log({run: $GT.run_id, step: 'summary-gagal', error: error});
  throw error;                                   // tandai job induk sebagai gagal
}

$GT.log({run: $GT.run_id, step: 'done'});
```

## Cara kerja

| Baris | Perilaku |
|---|---|
| `$GT.parallel([...])` | Menjalankan semua job anak bersamaan, lalu menunggu semuanya. Gagal pada salah satu membatalkan kelompok dan melempar error |
| `$GT.run('...', false)` | Menjalankan satu job dan **mengembalikan** teks error alih-alih melempar |
| `throw error` | Meneruskan error agar job induk ditandai gagal |

## Hasil

`build-summary` baru berjalan setelah `import-a` dan `import-b` sama-sama selesai.

## Catatan

* `$GT.parallel` **tidak** mengembalikan hasil tiap job, hanya `null` atau teks error.
* Jumlah job di `parallel` tidak boleh melebihi `maxConcurrentNestedJobs` di konfigurasi.
* Kedalaman nested maksimum **8**.
* `exception=false` hanya menekan error eksekusi. Job yang tidak ditemukan tetap melempar error.

**Referensi:** [Mengatur job di scheduler](../../amp/connection/javascript/methods/panduan/job-scheduler.md)
