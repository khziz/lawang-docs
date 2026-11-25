# Panduan per tugas

**Mode:** gateway. **Anda akan belajar:** membaca request, memeriksa input, dan membuat response.

## Langkah 1: Baca data dari request

```js
const payload = $GT.payload ? JSON.parse($GT.payload) : {};   // body JSON
const query   = $GT.queries ? JSON.parse($GT.queries) : {};   // ?page=2
const params  = $GT.params  ? JSON.parse($GT.params)  : {};   // /items/{id}
const headers = $GT.headers ? JSON.parse($GT.headers) : {};
```

Daftar lengkapnya ada di [Data yang tersedia di script](../methods/data-request.md).

Ingat: nilai `query` dan `headers` berupa **array**, jadi ambil elemen pertama.

```js
const page = query.page ? query.page[0] : '1';
```

## Langkah 2: Periksa input

Tolak request yang tidak lengkap sebelum bekerja lebih jauh.

```js
let result;
if (!payload.name) {
  result = $GT.response({message: 'name wajib diisi'}, 400);
} else {
  // ... lanjutkan pekerjaan, lalu isi result dengan response sukses
}
result;
```

{% hint style="warning" %}
Pada script biasa, response adalah **ekspresi terakhir**, jadi tidak ada `return` untuk berhenti lebih awal. Gunakan `if/else`, atau pakai **modul** (lihat langkah 5) agar bisa `return`.
{% endhint %}

## Langkah 3: Lakukan pekerjaan

Misalnya simpan ke database:

```js
$GT.save('db_main', 'items', [{id: $GT.uuid(), name: payload.name}]);
```

## Langkah 4: Buat response

```js
$GT.response({ok: true});                                   // 200 + JSON
$GT.response({id: 7}, 201);                                 // 201 Created
$GT.response('Halo', 200, 'text/plain');                    // teks biasa
$GT.response({ok: true}, 200, {'X-Trace': 'demo'});         // dengan header
$GT.response({ok: true}, 200, 'application/json',
  {'X-Trace': 'demo', 'Cache-Control': 'no-store'});        // content type + header
$GT.response('', 204);                                      // tanpa isi
```

Aturan argumen:

1. Body: object/array menjadi JSON, nilai lain menjadi teks.
2. Kode status (default `200`).
3. Content type **atau** object header.
4. Header (hanya dipakai jika argumen ketiga **bukan** object).

## Langkah 5: Susun dengan benar

### Pola script biasa

```js
const q = $GT.queries ? JSON.parse($GT.queries) : {};
const id = q.id ? Number(q.id[0]) : 7;
let result;
try {
  const rows = $GT.query('db_main',
    'SELECT id, name FROM items WHERE id = ?', [id], 5);
  result = $GT.response({data: rows}, 200, 'application/json');
} catch (err) {
  $GT.log(String(err));
  result = $GT.response({message: 'Operasi gagal'}, 500);
}
result;   // baris terakhir = response
```

### Pola modul (bisa `return` kapan saja)

```js
export default function () {
  const payload = $GT.payload ? JSON.parse($GT.payload) : {};
  if (!payload.name) {
    return $GT.response({message: 'name wajib diisi'}, 400);
  }
  $GT.save('db_main', 'items', [{id: $GT.uuid(), name: payload.name}]);
  return $GT.response({ok: true}, 201);
}
```

## Tips

* Jangan kirim pesan error teknis mentah ke pemanggil. Catat dengan `$GT.log`, balas pesan umum.
* Gunakan `$GT.uuid()` untuk ID baru.
* Mau menerima file unggahan? Lihat [Bekerja dengan file](bekerja-dengan-file.md#menyimpan-file-yang-diunggah).

**Referensi:** [HTTP & respons](../ringkasan-method/http.md)
