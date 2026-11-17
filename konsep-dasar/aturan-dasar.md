# Aturan dasar skrip

Delapan hal ini berlaku untuk **hampir semua** skrip. Membacanya sekali di awal akan menghindarkan banyak kebingungan.

## 1. Tulis `$GT.nama` secara langsung

Lawang memuat fungsi berdasarkan teks `$GT.nama` yang tertulis di skrip (dan di modul yang diimpor).

```js
$GT.query('db_main', 'SELECT 1');   // benar

const nama = 'query';
$GT[nama]('db_main', 'SELECT 1');   // salah: fungsi bisa tidak dimuat
```

{% hint style="warning" %}
Hindari `$GT[nama]` dan destructuring seperti `const {query} = $GT`. Fungsi bisa tidak dimuat.
{% endhint %}

`$GT.run` dan `$GT.parallel` tidak terkena aturan ini karena dipasang langsung oleh scheduler.

## 2. Semua fungsi langsung selesai (tanpa Promise)

Fungsi `$GT` mengembalikan hasilnya langsung. **Jangan** memakai `await` atau `.then()`.

```js
const rows = $GT.query('db_main', 'SELECT id FROM items');  // hasil langsung ada
```

Runtime dibuat baru di setiap eksekusi. Tidak ada event loop seperti di Node.js.

## 3. Tidak ada `fetch`, `setTimeout`, `require`, dan kawan-kawannya

Pakai pengganti dari Lawang:

| Anda mungkin ingin | Gunakan                          |
| ------------------ | -------------------------------- |
| `fetch(url)`       | `$GT.get(url)` / `$GT.post(...)` |
| `Buffer` / base64  | `btoa()` / `atob()`              |
| Membaca file       | `$GT.load_file(...)`             |

## 4. Nama koneksi berasal dari layanan pusat

Saat Anda menulis `'db_main'`, `'files'`, atau `'app-secret'`, itu adalah **nama** yang harus sudah terdaftar di layanan pusat. Skrip tidak pernah menyimpan alamat database atau password.

Jika nama tidak ada, fungsi akan melempar error.

## 5. Gateway dan scheduler itu terpisah

* `$GT.response` hanya jalan di **gateway**.
* `$GT.run`, `parallel`, `sync`, `rfc_sync` hanya jalan di **scheduler**.

## 6. Satuan timeout adalah detik

Detail lengkap ada di [Error dan timeout](error-dan-timeout.md). Singkatnya: pakai angka positif.

## 7. Error berupa teks

Error umumnya dilempar sebagai **string**, bukan object `Error`. Tangkap dengan `try/catch` dan ubah ke teks dengan `String(err)`.

## 8. Modul

Skrip boleh berupa modul:

* `import` statis dari lokasi yang diizinkan, **dynamic import ditolak**.
* `export default` harus berupa **function**.
* Batas: **1 MiB** per file sumber, **8 MiB** total, **128** modul.

```js
export default function () {
  return $GT.response({ok: true});
}
```
