# Kasus 3: CRUD items dengan JavaScript

## Skenario

Anda butuh API `items` dengan logika sendiri: membaca satu item, dan membuat item baru dengan validasi. Tidak ada upstream, semuanya ditulis dengan script.

## Komponen

| Komponen | Nilai contoh |
|---|---|
| Connection JavaScript | `js-main` |
| Endpoint 1 | `/items` GET (baca) |
| Endpoint 2 | `/items` POST (buat) |
| Service | `/api/items` (untuk masing-masing method) |

Database `db_main` dan tabel `items` (kolom `id`, `name`) sudah terdaftar di konfigurasi Anda.

## Langkah

1. Buat Connection JavaScript `js-main`.
2. Buat dua endpoint dengan connection type `JavaScript`. Satu endpoint per HTTP method.
3. Isi script masing-masing (di bawah).
4. Buat service untuk tiap endpoint.

## Script GET: baca satu item

```js
const q = $GT.queries ? JSON.parse($GT.queries) : {};
const id = q.id ? Number(q.id[0]) : null;
let result;

if (id === null || isNaN(id)) {
  result = $GT.response({message: 'id wajib berupa angka'}, 400);
} else {
  try {
    const rows = $GT.query('db_main',
      'SELECT id, name FROM items WHERE id = ?', [id], 5);
    result = rows.length > 0
      ? $GT.response({data: rows[0]}, 200)
      : $GT.response({message: 'Item tidak ditemukan'}, 404);
  } catch (err) {
    $GT.log(String(err));
    result = $GT.response({message: 'Operasi gagal'}, 500);
  }
}
result;   // baris terakhir = response
```

## Script POST: buat item baru

Memakai bentuk **module** agar bisa `return` lebih awal.

```js
export default function () {
  const payload = $GT.payload ? JSON.parse($GT.payload) : {};

  if (!payload.name) {
    return $GT.response({message: 'name wajib diisi'}, 400);
  }

  const id = $GT.uuid();
  try {
    $GT.save('db_main', 'items', [{id: id, name: payload.name}], 5);
  } catch (err) {
    $GT.log(String(err));
    return $GT.response({message: 'Operasi gagal'}, 500);
  }

  return $GT.response({id: id}, 201);
}
```

## Hasil

| Request | Response |
|---|---|
| `GET ...?id=7` | `200` dan data item, atau `404` bila tidak ada |
| `GET` tanpa `id` | `400` |
| `POST` dengan `{"name": "A"}` | `201` dan `{"id": "<uuid>"}` |
| `POST` tanpa `name` | `400` |

## Catatan

* Kolom `id` bertipe teks (UUID) pada contoh ini. Sesuaikan dengan skema tabel Anda.
* Method yang dipakai: [`$GT.query`](../connection/javascript/methods/referensi/database.md), [`$GT.save`](../connection/javascript/methods/referensi/database.md), [`$GT.response`](../connection/javascript/methods/referensi/http.md), [`$GT.uuid`](../connection/javascript/methods/referensi/utilitas-logging.md).
* Error teknis hanya dicatat dengan `$GT.log`. Consumer menerima pesan umum.
