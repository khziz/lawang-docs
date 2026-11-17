# Koneksi JavaScript

Berbeda dari connection HTTP dan database, connection **JavaScript tidak memerlukan upstream**. Seluruh logika request dan response ditulis dengan **skrip JavaScript**.

AMP sudah menyediakan **fungsi-fungsi JavaScript siap pakai** (kotak peralatan `$GT`) untuk mempermudah penulisan skrip: membaca database, memanggil API lain, mengolah file, enkripsi, dan banyak lagi.

## 1. Connection JavaScript

Buat connection bertipe JavaScript. Tidak ada alamat upstream yang perlu diisi.

## 2. Endpoint JavaScript

| Field                 | Isi                                                                  |
| --------------------- | -------------------------------------------------------------------- |
| **Connection type**   | Pilih `JavaScript`                                                   |
| **Connection**        | Pilih connection yang sudah dibuat (otomatis difilter ke JavaScript) |
| **API Path**          | Path bebas, misalnya `/get/js`                                       |
| **Access level**      | `public`, `private`, atau `user`                                     |
| **HTTP method**       | `GET`, `POST`, `PUT`, `PATCH`, `DELETE`                              |
| **Skip activity log** | Tidak mencatat log endpoint ini ke Elasticsearch                     |
| **Script**            | Editor tempat Anda menulis skrip JavaScript                          |

### Konfigurasi lanjutan

* request timeout
* rate limit
* request transformer
* log transformer
* request validation
* CORS

{% hint style="warning" %}
Endpoint JavaScript **tidak** memiliki response transformer. Atur jawaban lewat `$GT.response(...)` di dalam skrip.
{% endhint %}

## 3. Service JavaScript

| Field               | Isi                          |
| ------------------- | ---------------------------- |
| **Path**            | Bebas                        |
| **Connection type** | `JavaScript`                 |
| **Connection**      | Connection yang sudah dibuat |
| **Endpoint**        | Endpoint yang sudah dibuat   |

## Contoh skrip sederhana

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
result;   // baris terakhir = jawaban endpoint
```

## Belajar menulis skrip

Panduan lengkap fungsi JavaScript ada di bagian berikut, dengan urutan belajar dari dasar:

| Anda ingin...              | Buka                                                                |
| -------------------------- | ------------------------------------------------------------------- |
| Mengenal skrip dan `$GT`   | [Mengenal skrip JavaScript](../mulai-di-sini/mengenal-skrip.md)     |
| Mencoba skrip pertama      | [Skrip pertama Anda](../mulai-di-sini/skrip-pertama.md)             |
| Memahami aturan dasar      | [Aturan dasar skrip](../konsep-dasar/aturan-dasar.md)               |
| Mengerjakan tugas tertentu | [Panduan per tugas](../amp-panduan-fungsi-javascript/javascript.md) |
| Mencari detail satu fungsi | [Ringkasan semua fungsi](../referensi-fungsi/ringkasan-fungsi.md)   |
| Memecahkan error           | [Pemecahan masalah](../contoh-dan-bantuan/pemecahan-masalah.md)     |

{% hint style="info" %}
Fungsi yang sama juga dipakai pada **job di DSP** (mode scheduler). Lihat [DSP: Pengantar](../dsp-data-synchronization/dsp.md).
{% endhint %}
