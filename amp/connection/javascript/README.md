---
description: "Connection JavaScript di AMP, endpoint tanpa upstream yang logikanya ditulis dengan script."
---

# Connection JavaScript

Berbeda dari Connection HTTP dan Database, Connection **JavaScript tidak memerlukan upstream**. Seluruh logika request dan response ditulis dengan **script JavaScript**.

AMP menyediakan kumpulan **method siap pakai** (kotak peralatan `$GT`) untuk mempermudah penulisan script: membaca database, memanggil API lain, mengolah file, enkripsi, dan banyak lagi. Kumpulan ini dibahas di halaman [Method JavaScript](methods/README.md).

## 1. Connection JavaScript

Buat connection bertipe JavaScript. Tidak ada alamat upstream yang perlu diisi.

## 2. Endpoint JavaScript

| Field | Isi |
|---|---|
| **Connection type** | Pilih `JavaScript` |
| **Connection** | Pilih connection yang sudah dibuat (otomatis difilter ke JavaScript) |
| **API Path** | Path bebas, misalnya `/get/js` |
| **Access level** | `public`, `private`, atau `user` |
| **HTTP method** | `GET`, `POST`, `PUT`, `PATCH`, `DELETE` |
| **Skip activity log** | Tidak mencatat log endpoint ini ke Elasticsearch |
| **Script** | Editor tempat Anda menulis script JavaScript |

### Advanced configuration

* request timeout
* rate limit
* request transformer
* log transformer
* request validation
* CORS

{% hint style="warning" %}
Endpoint JavaScript **tidak** memiliki response transformer. Atur response lewat `$GT.response(...)` di dalam script.
{% endhint %}

## 3. Service JavaScript

| Field | Isi |
|---|---|
| **Path** | Bebas |
| **Connection type** | `JavaScript` |
| **Connection** | Connection yang sudah dibuat |
| **Endpoint** | Endpoint yang sudah dibuat |

## Contoh script sederhana

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
result;   // baris terakhir = response endpoint
```

## Selanjutnya: Method JavaScript

Panduan lengkap method `$GT` ada di [Method JavaScript](methods/README.md). Urutan belajarnya:

| Anda ingin... | Buka |
|---|---|
| Mengenal script dan `$GT` | [Mengenal script JavaScript](methods/mulai/mengenal-script.md) |
| Mencoba script pertama | [Script pertama Anda](methods/mulai/script-pertama.md) |
| Memahami aturan dasar | [Aturan dasar script](methods/konsep/aturan-dasar.md) |
| Mengerjakan tugas tertentu | [Panduan per tugas](methods/README.md) |
| Mencari detail satu method | [Ringkasan semua method](methods/referensi/ringkasan-method.md) |
| Memecahkan error | [Troubleshooting](methods/bantuan/pemecahan-masalah.md) |
| Melihat contoh kasus utuh | [Study Case AMP](../../study-case/README.md) |

{% hint style="info" %}
Method yang sama juga dipakai pada **job di DSP** (mode scheduler). Lihat [DSP: Overview](../../../dsp/README.md).
{% endhint %}
