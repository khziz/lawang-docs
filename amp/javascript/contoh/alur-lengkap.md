# Contoh alur lengkap

## 1. Endpoint gateway (skrip biasa)

Nilai **ekspresi terakhir** adalah object respons yang diterima compiler.

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
result;
```

Alurnya:

1. Baca query string `id` (default 7).
2. Jalankan query dengan parameter dan timeout 5 detik.
3. Jika berhasil, kembalikan data dengan status 200.
4. Jika gagal, catat error ke log dan kembalikan pesan umum dengan status 500.

## 2. Endpoint gateway (modul)

Respons dikembalikan lewat `return` pada **default export**.

```js
export default function () {
  const payload = $GT.payload ? JSON.parse($GT.payload) : {};
  return $GT.response({received: payload, id: $GT.uuid()}, 200);
}
```

## 3. Orkestrasi scheduler

Child job berjalan sesuai kapasitas scheduler. Langkah terakhir dimulai setelah kedua import selesai.

```js
$GT.log({run: $GT.run_id, step: 'start'});
$GT.parallel(['import-a', 'import-b']);
const error = $GT.run('build-summary', false);
if (error !== null) { throw error; }
$GT.log({run: $GT.run_id, step: 'done'});
```
