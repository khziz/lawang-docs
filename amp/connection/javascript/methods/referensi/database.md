# Database

{% hint style="success" %}
Baru belajar? Baca dulu [panduan langkah demi langkah](../panduan/bekerja-dengan-database.md). Halaman ini adalah **referensi teknis** untuk dicari ulang.
{% endhint %}

Enam method untuk membaca dan menulis database: `query`, `exec`, `exists`, `save`, `update`, `upsert`.

## Aturan umum

Berlaku untuk semua method di halaman ini. **Mode:** gateway dan scheduler.

* `source` adalah **nama backend database** dari konfigurasi pusat, bukan DSN.
* `timeout` dalam detik. `0` atau nilai melebihi `RequestTimeout` memakai `RequestTimeout`.
* Gagal connection atau operasi umumnya **melempar exception**.
* Placeholder SQL mengikuti driver. Contoh `?` perlu disesuaikan untuk PostgreSQL.

{% hint style="warning" %}
Selalu gunakan **parameter** (`?`) untuk nilai dari input pengguna, jangan menyambung string SQL.
{% endhint %}

## Pilih method yang tepat

| Kebutuhan | Method |
|---|---|
| Mengambil data | `query` |
| SQL bebas tanpa hasil (INSERT/UPDATE/DELETE) | `exec` |
| Cek apakah data ada | `exists` |
| Menyimpan satu/banyak record baru | `save` |
| Memperbarui satu record | `update` |
| Insert-atau-update banyak record | `upsert` |

## `$GT.query`

```js
$GT.query(source, sql, parameters?, timeout?)
```

| Parameter | Tipe | Keterangan |
|---|---|---|
| `source` | string | Nama backend database |
| `sql` | string | Query SQL |
| `parameters` | array / string JSON array / integer | Integer dianggap timeout tanpa parameter |
| `timeout` | integer | Detik |

**Hasil:** `Array<object>`. Kosong menjadi `[]`. **Maksimum 1.000 baris.**

**Konversi tipe data**

| Dari database | Menjadi |
|---|---|
| `NULL` | `null` |
| byte | string |
| waktu | string RFC3339Nano |
| `int64` | number (nilai di luar rentang integer aman JS dapat **kehilangan presisi**) |

```js
$GT.query('db_main', 'SELECT id, name FROM items');
$GT.query('db_main', 'SELECT name FROM items WHERE id = ?', [7]);
$GT.query('db_main', 'SELECT name FROM items WHERE id = ?', [7], 10);
$GT.query('db_main', 'SELECT name FROM items WHERE id = ?', '[7]', 10);
$GT.query('db_main', 'SELECT id FROM items', 5);       // timeout singkat
$GT.query('db_main', 'SELECT id FROM items', [], 5);
```

## `$GT.exec`

Menjalankan SQL yang tidak mengembalikan baris.

```js
$GT.exec(source, sql, parameters?, timeout?)
```

Parameter sama dengan `query`. **Hasil:** `null` saat sukses. Tidak mengembalikan jumlah baris terpengaruh maupun insert ID.

```js
$GT.exec('db_main', 'DELETE FROM staging WHERE processed = 1');
$GT.exec('db_main', 'DELETE FROM items WHERE id = ?', [7]);
$GT.exec('db_main', 'UPDATE items SET name = ? WHERE id = ?',
  ['Baru', 7], 10);
$GT.exec('db_main', 'DELETE FROM items WHERE id = ?', '[7]', 5);
$GT.exec('db_main', 'DELETE FROM staging WHERE processed = 1', 5);
```

## `$GT.exists`

Memeriksa apakah ada baris dengan kolom dan nilai tertentu.

```js
$GT.exists(source, table, payload, timeout?)
```

| Parameter | Tipe | Keterangan |
|---|---|---|
| `table` | string | Nama tabel |
| `payload` | object | Pasangan kolom dan nilai pencarian. **Harus object**, bukan string JSON |
| `timeout` | integer | Detik |

**Hasil:** `boolean`.

{% hint style="warning" %}
`false` tidak selalu berarti "data tidak ditemukan". Bisa juga pemeriksaannya gagal.
{% endhint %}

```js
$GT.exists('db_main', 'items', {id: 7});
$GT.exists('db_main', 'items', {code: 'A', active: true}, 10);
if (!$GT.exists('db_main', 'items', {id: 7}, 5)) {
  $GT.log('Data tidak ditemukan atau pemeriksaan gagal');
}
```

## `$GT.save`

Menyimpan record ke tabel.

```js
$GT.save(source, table, records, timeout?)
```

| Parameter | Tipe | Keterangan |
|---|---|---|
| `records` | `Array<object>` | **Selalu array**, termasuk untuk satu record |

**Hasil:** `null` saat sukses. Tidak mengembalikan jumlah baris. Object tunggal tidak diterima sebagai pengganti array.

```js
$GT.save('db_main', 'items', [{id: 7, name: 'A'}]);
$GT.save('db_main', 'items',
  [{id: 8, name: 'B'}, {id: 9, name: 'C'}], 10);
try {
  $GT.save('db_main', 'items', [{id: 7}], 5);
} catch (err) { $GT.log(String(err)); }
```

## `$GT.update`

Memperbarui **satu** record.

```js
$GT.update(source, table, identities, payload, timeout?)
```

| Parameter | Tipe | Keterangan |
|---|---|---|
| `identities` | `string[]` | **Nama kolom** identitas (bukan nilai ID) |
| `payload` | object | Nilai identitas dan field yang diperbarui. Satu object, bukan array |

**Hasil:** `null` saat sukses.

```js
$GT.update('db_main', 'items', ['id'], {id: 7, name: 'Baru'});
$GT.update('db_main', 'items', ['tenant_id', 'code'],
  {tenant_id: 1, code: 'A', name: 'Baru'}, 10);
```

## `$GT.upsert`

Insert atau update banyak record sekaligus.

```js
$GT.upsert(source, table, identities, records, timeout?)
```

| Parameter | Tipe | Keterangan |
|---|---|---|
| `identities` | `string[]` | Nama kolom identitas. Driver mencari constraint berdasarkan ini |
| `records` | `Array<object>` | Daftar record |

**Hasil:** `null` saat sukses. Kemampuan insert/update dan constraint mengikuti backend database.

```js
$GT.upsert('db_main', 'items', ['id'], [{id: 7, name: 'A'}]);
$GT.upsert('db_main', 'items', ['id'],
  [{id: 7, name: 'Baru'}, {id: 8, name: 'B'}], 10);
$GT.upsert('db_main', 'items', ['tenant_id', 'code'],
  [{tenant_id: 1, code: 'A', name: 'A'}], 5);
```
