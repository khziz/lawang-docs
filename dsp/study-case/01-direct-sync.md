# Kasus 1: Sinkronisasi database ke database setiap malam

## Skenario

Data `items` di database operasional (`DB-SOURCE`) perlu disalin ke database laporan (`DB-TARGET`) setiap malam. Data yang sudah ada harus diperbarui, bukan diduplikasi.

## Komponen

| Komponen | Nilai contoh |
|---|---|
| Server | `scheduler-01` (ditandai sebagai scheduler) |
| Schedule | `nightly-1am`, cron `0 1 * * *` |
| Job | `sync-items` |
| Metode | **Direct** dengan **upsert** |

## Langkah

1. **Server:** menu Server → Create new → isi name dan host → aktifkan **Set as scheduler** → Submit.
2. **Schedule:** isi name dan description → atur cron (contoh `0 1 * * *`, setiap hari pukul 01:00) → gunakan **test syntax** → Submit.
3. **Job:** isi name `sync-items` → atur timeout (opsional) → pilih schedule dan server → isi script → Submit.

{% hint style="info" %}
Cron `0 1 * * *` adalah contoh umum. Gunakan fitur **test syntax** untuk memastikan formatnya diterima.
{% endhint %}

## Script

```js
$GT.log({run: $GT.run_id, step: 'start'});

$GT.sync({
  connection: "DB-SOURCE",
  query: "SELECT * FROM items"
}, {
  connection: "DB-TARGET",
  table: "items",
  identityFields: ["id"]
}, {"worker": 4, "batchSize": 1000});

$GT.log({run: $GT.run_id, step: 'done'});
```

| Bagian | Fungsi |
|---|---|
| `query` | Data apa yang diambil dari source |
| `table` | Tabel tujuan |
| `identityFields` | Kolom penentu baris yang sama. Karena diisi, mode-nya **upsert** |
| `worker`, `batchSize` | Jumlah worker paralel dan ukuran batch (1 sampai 10000) |

## Hasil

Setiap pukul 01:00, baris dengan `id` yang sudah ada diperbarui, dan baris baru ditambahkan di `DB-TARGET`. Log `start` dan `done` tercatat di **Scheduler Log**.

## Variasi

### Insert saja (tanpa upsert)

Hapus `identityFields`.

```js
$GT.sync({
  connection: "DB-SOURCE",
  query: "SELECT * FROM items"
}, {
  connection: "DB-TARGET",
  table: "items"
}, {"worker": 4, "batchSize": 1000});
```

### Ambil sebagian data dengan parameter

```js
$GT.sync({
  connection: "DB-SOURCE",
  query: "SELECT * FROM items WHERE id > ?",
  parameters: [10]
}, {
  connection: "DB-TARGET",
  table: "items",
  identityFields: ["id"]
});
```

Placeholder `?` mengikuti driver database Anda.

**Referensi:** [`$GT.sync`](../../amp/connection/javascript/methods/referensi/scheduler.md)
