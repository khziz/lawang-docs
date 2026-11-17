# Kasus 2: Upsync, cabang ke pusat

## Skenario

Setiap kantor cabang punya database lokal yang dijangkau lewat **agent** bernama `branch`. Data `items` cabang perlu dikirim ke database pusat.

## Komponen

| Komponen | Nilai contoh |
|---|---|
| Agent | `branch` |
| Source | Database cabang, connection `local_db` (lewat agent) |
| Target | Database pusat, connection `db_b` |
| Metode | **Upsync** dengan upsert |

## Aturan penting

Pada Upsync, `agent` ditulis di **source**. Jangan isi `agent` di target.

## Script

```js
$GT.sync({
  agent: 'branch',
  connection: 'local_db',
  query: 'SELECT id, name FROM items'
}, {
  connection: 'db_b',
  table: 'items',
  identityFields: ['id']
}, {worker: 2});
```

## Hasil

Data dari database cabang disalin ke tabel `items` di pusat. Baris dengan `id` sama diperbarui, baris baru ditambahkan.

## Catatan

* Hapus `identityFields` bila Anda hanya ingin menambah data (insert).
* Agar data dari beberapa cabang tidak saling menimpa, pastikan kolom identitas unik lintas cabang (misalnya kombinasi `branch_id` dan `id`). Contoh: `identityFields: ['branch_id', 'id']`.
* Gunakan `agent` di **satu sisi saja**.

**Referensi:** [Fitur sinkronisasi data](../sinkronisasi.md)
