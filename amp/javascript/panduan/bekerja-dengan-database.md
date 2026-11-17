# Bekerja dengan database

**Mode:** gateway dan scheduler. **Anda akan belajar:** membaca dan menulis data dengan aman.

Semua fungsi memakai **nama backend database** (misalnya `'db_main'`) sebagai argumen pertama. Alamat dan password tersimpan di layanan pusat.

## Langkah 1: Pilih fungsi yang tepat

| Anda ingin... | Pakai |
|---|---|
| Mengambil data | `$GT.query` |
| Menjalankan SQL bebas (UPDATE/DELETE massal, dsb.) | `$GT.exec` |
| Mengecek apakah data ada | `$GT.exists` |
| Menambah data baru | `$GT.save` |
| Mengubah **satu** data | `$GT.update` |
| Tambah-atau-ubah banyak data | `$GT.upsert` |

## Langkah 2: Membaca data

```js
const rows = $GT.query('db_main',
  'SELECT id, name FROM items WHERE id = ?', [7], 10);
```

* Isi tanda `?` dari array parameter. **Jangan** menyambung SQL dengan teks dari input pengguna.
* Hasilnya array of object: `[{id: 7, name: 'A'}]`. Kosong menjadi `[]`.
* **Maksimum 1.000 baris.** Untuk data lebih banyak, batasi query atau ambil bertahap lewat SQL (cara paging mengikuti database Anda).
* Placeholder bisa berbeda per database (misalnya PostgreSQL tidak memakai `?`). Sesuaikan dengan driver Anda.

Pembacaan nilai:

| Dari database | Di JavaScript |
|---|---|
| `NULL` | `null` |
| Waktu | Teks format RFC3339Nano |
| Byte | Teks |
| Angka `int64` | number (angka sangat besar bisa kehilangan presisi) |

## Langkah 3: Menulis data

### Tambah data

`save` selalu menerima **array**, walau hanya satu record.

```js
$GT.save('db_main', 'items', [{id: 7, name: 'A'}]);
$GT.save('db_main', 'items', [{id: 8, name: 'B'}, {id: 9, name: 'C'}], 10);
```

### Ubah satu data

`identities` berisi **nama kolom** penentu baris, bukan nilainya.

```js
$GT.update('db_main', 'items', ['id'], {id: 7, name: 'Baru'});
$GT.update('db_main', 'items', ['tenant_id', 'code'],
  {tenant_id: 1, code: 'A', name: 'Baru'});
```

### Tambah atau ubah banyak data (upsert)

```js
$GT.upsert('db_main', 'items', ['id'],
  [{id: 7, name: 'Baru'}, {id: 8, name: 'B'}], 10);
```

Baris dengan `id` yang sudah ada diperbarui, yang belum ada ditambahkan. Bisa tidaknya bergantung pada constraint di database Anda.

### SQL bebas

```js
$GT.exec('db_main', 'UPDATE items SET name = ? WHERE id = ?', ['Baru', 7], 10);
$GT.exec('db_main', 'DELETE FROM staging WHERE processed = 1');
```

## Langkah 4: Mengecek keberadaan data

```js
if ($GT.exists('db_main', 'items', {id: 7})) {
  // ada
}
```

{% hint style="warning" %}
`false` bisa berarti "tidak ada" **atau** "pemeriksaan gagal". Jika perbedaannya penting, gunakan `$GT.query`.
{% endhint %}

## Langkah 5: Tangani error

```js
try {
  $GT.save('db_main', 'items', [{id: 7}], 5);
} catch (err) {
  $GT.log(String(err));
}
```

Fungsi tulis (`exec`, `save`, `update`, `upsert`) hanya mengembalikan `null` bila sukses. Jumlah baris terpengaruh tidak dikembalikan.

**Referensi:** [Database](../referensi/database.md)
