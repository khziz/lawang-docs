# Error dan timeout

## Dua jenis "gagal"

Penting dibedakan:

| Jenis | Contoh | Perilaku |
|---|---|---|
| **Gagal teknis** | Connection putus, SQL salah, nama backend tidak ada | Method **melempar exception** |
| **Gagal HTTP** | API lain merespons `404` atau `500` | Method **tidak** melempar. Anda cek sendiri `code` |

```js
const res = $GT.get('https://api.example.com/items/7', 5);
if (res.code !== 200) {
  $GT.log({status: res.code});
}
```

## Menangkap error

Error umumnya berupa **teks** (string). Pakai pola ini:

```js
try {
  const rows = $GT.query('db_main', 'SELECT id FROM items', [], 5);
  $GT.log({count: rows.length});
} catch (err) {
  $GT.log(typeof err === 'object' ? err : String(err));
}
```

`typeof err === 'object'` dipakai karena error **RFC (SAP)** bisa berupa object dengan rincian lebih lengkap.

### Field pada error RFC

| Field | Kegunaan |
|---|---|
| `type`, `category`, `operation` | Jenis dan tahap yang gagal |
| `summary`, `message` | Penjelasan |
| `code`, `key` | Kode dan kunci terkait |
| `suggestion` | Saran perbaikan |
| `abap_class`, `abap_type`, `abap_number` | Detail dari sisi ABAP |
| `retryable` | Apakah layak dicoba ulang |

Sebagian field bisa tidak ada.

## Timeout

* Satuannya **detik**.
* Untuk HTTP, database, dan RFC: `0` atau angka lebih besar dari `RequestTimeout` otomatis **diganti** nilai konfigurasi.
* Method file tidak punya parameter timeout dan selalu memakai `RequestTimeout`.

{% hint style="danger" %}
Jangan memakai timeout **negatif** untuk "tanpa batas". Itu tidak aman: batas dari context induk tetap berlaku. Selalu pakai angka **positif**.
{% endhint %}

### Cara memasang timeout

Biasanya timeout adalah argumen **terakhir**:

```js
$GT.query('db_main', 'SELECT id FROM items', [], 5);       // 5 detik
$GT.get('https://api.example.com/items', {Accept: 'application/json'}, 10);
```

Beberapa method mengizinkan timeout "singkat" di slot sebelumnya (`$GT.get(url, 5)`), tapi agar jelas, taruh timeout di posisi paling akhir.
