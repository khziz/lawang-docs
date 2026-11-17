# Script pertama Anda

Di halaman ini Anda menulis tiga script kecil, dari yang paling sederhana.

## Script 1: Response sederhana (gateway)

```js
$GT.response({pesan: 'Halo dari Lawang'});
```

Hasilnya: pemanggil endpoint menerima JSON `{"pesan":"Halo dari Lawang"}` dengan status `200`.

{% hint style="info" %}
Pada script biasa, **baris terakhir** yang menghasilkan `$GT.response(...)` menjadi response endpoint.
{% endhint %}

## Script 2: Membaca input dan database

Misalnya endpoint dipanggil dengan `?id=7`. Script berikut mengambil data item itu.

```js
// 1. Ambil query string (nilainya berupa array)
const q = $GT.queries ? JSON.parse($GT.queries) : {};
const id = q.id ? Number(q.id[0]) : 7;   // default 7 bila tidak diisi

// 2. Siapkan variabel hasil
let result;

// 3. Coba baca database, tangani kegagalan
try {
  const rows = $GT.query('db_main',
    'SELECT id, name FROM items WHERE id = ?', [id], 5);
  result = $GT.response({data: rows}, 200, 'application/json');
} catch (err) {
  $GT.log(String(err));
  result = $GT.response({message: 'Operasi gagal'}, 500);
}

// 4. Baris terakhir = response endpoint
result;
```

Apa yang terjadi, langkah demi langkah:

1. `$GT.queries` berisi query string dalam bentuk teks JSON, jadi di-`JSON.parse` dulu.
2. `$GT.query` menjalankan SQL. Tanda `?` diisi dari array `[id]`, sehingga aman dari penyusupan SQL. Angka `5` adalah timeout (detik).
3. `try/catch` menangkap error. Kegagalan dicatat dengan `$GT.log`, dan pemanggil hanya melihat pesan umum.
4. `result;` di baris akhir adalah response yang dikirim.

## Script 3: Job terjadwal (scheduler)

```js
$GT.log({run: $GT.run_id, step: 'start'});
$GT.parallel(['import-a', 'import-b']);          // jalankan dua job bersamaan
const error = $GT.run('build-summary', false);   // lalu satu job lagi
if (error !== null) { throw error; }
$GT.log({run: $GT.run_id, step: 'done'});
```

Alurnya: catat awal, jalankan `import-a` dan `import-b` **bersamaan** dan tunggu keduanya selesai, jalankan `build-summary`, lalu catat selesai. Jika `build-summary` gagal, error dilempar ulang supaya job ini ditandai gagal.

## Selanjutnya

* Pahami aturan yang berlaku untuk semua script: [Aturan dasar script](../konsep/aturan-dasar.md).
* Bingung dengan istilah? Lihat [Istilah penting](istilah.md).
