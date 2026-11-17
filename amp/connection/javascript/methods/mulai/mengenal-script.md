# Mengenal script JavaScript

Lawang dapat menjalankan **script JavaScript** yang Anda tulis. Script itu bisa membaca database, memanggil API lain, mengolah file, dan mengirim response, tanpa Anda membuat server sendiri.

Script JavaScript dipakai di **dua tempat**, dan keduanya memakai kotak peralatan yang sama (`$GT`):

| Mode | Dipakai di | Cara membuatnya | Tugas umum |
|---|---|---|---|
| **Gateway** | [AMP](../../../../README.md): connection **JavaScript** | Buat endpoint bertipe JavaScript, lalu isi script-nya | Membuat endpoint API |
| **Scheduler** | [DSP](../../../../../dsp/README.md): **Job** | Buat job, lalu isi kolom script | Impor data, sinkronisasi, tugas berkala |

| | **Gateway (AMP)** | **Scheduler (DSP)** |
|---|---|---|
| Dipicu oleh | Request HTTP yang masuk | Jadwal atau perintah job |
| Merespons dengan | `$GT.response(...)` | Tidak ada response, hanya menjalankan pekerjaan |
| Method khusus | `response` | `run`, `parallel`, `sync`, `rfc_sync` |

Sebagian besar method bisa dipakai di **kedua mode**. Method khusus satu mode ditandai jelas di dokumentasi ini.

## Kotak peralatan: `$GT`

Semua kemampuan Lawang ada di satu object bernama **`$GT`**:

```js
$GT.query('db_main', 'SELECT id, name FROM items');   // baca database
$GT.get('https://api.example.com/items');             // panggil API lain
$GT.uuid();                                           // buat ID unik
```

| Kelompok | Contoh method |
|---|---|
| Utilitas | `log`, `uuid` |
| HTTP | `get`, `post`, `put`, `patch`, `delete`, `request`, `response` |
| Database | `query`, `exec`, `exists`, `save`, `update`, `upsert` |
| File | `load_file`, `store_file`, `read_csv`, `read_excel` |
| Keamanan | `sha256`, `hmac256`, `encrypt`, `jwtrs256_signing` |
| Ubah data | `mask`, `xml2json`, `transform` |
| Pesan & SAP | `publish`, `rfc` |
| Scheduler | `run`, `parallel`, `sync`, `rfc_sync` |

## Bukan Node.js, bukan browser

Script berjalan di engine bernama **Sobek**. Ia memahami JavaScript standar (`JSON`, `Math`, `Date`, `Array`, dan sebagainya), tetapi **tidak** punya:

`fetch`, `setTimeout`, `setInterval`, `require`, `process`, `Buffer`, `window`, `document`

Sebagai gantinya, gunakan method `$GT`. Misalnya `$GT.get` untuk memanggil API, bukan `fetch`.

Berikutnya: [Script pertama Anda](script-pertama.md).
