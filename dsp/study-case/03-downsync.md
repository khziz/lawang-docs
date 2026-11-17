# Kasus 3: Downsync, master data pusat ke cabang

## Skenario

Master data produk dikelola di database pusat. Setiap cabang perlu salinan terbaru di database lokalnya, yang dijangkau lewat agent `branch`.

## Komponen

| Komponen | Nilai contoh |
|---|---|
| Agent | `branch` |
| Source | Database pusat, connection `db_a` |
| Target | Database cabang, connection `local_db` (lewat agent) |
| Metode | **Downsync** dengan upsert |

## Aturan penting

Pada Downsync, `agent` ditulis di **target**. Jangan isi `agent` di source.

## Script

```js
$GT.sync({
  connection: 'db_a',
  query: 'SELECT id, name FROM products'
}, {
  agent: 'branch',
  connection: 'local_db',
  table: 'products',
  identityFields: ['id']
}, {batchSize: 100});
```

## Hasil

Tabel `products` di cabang sama dengan di pusat. Baris dengan `id` sama diperbarui, baris baru ditambahkan.

## Catatan

* `batchSize` kecil (di sini 100) dapat berguna bila connection ke cabang lambat.
* Contoh Downsync pada materi presentasi menaruh `agent` di source. Di sini ditaruh di **target**, sesuai dokumentasi method `$GT.sync`. Mohon dicek di lingkungan Anda.

**Referensi:** [Fitur sinkronisasi data](../sinkronisasi.md)
