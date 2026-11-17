# Masking & transformasi

{% hint style="success" %}
Baru belajar? Baca dulu [panduan langkah demi langkah](../panduan/ubah-dan-samarkan-data.md). Halaman ini adalah **referensi teknis** untuk dicari ulang.
{% endhint %}

**Mode:** gateway dan scheduler, untuk semua method di halaman ini.

## Masking

Tiga method untuk menyamarkan sebagian string (misalnya nomor kartu atau telepon).

{% hint style="warning" %}
Semua method masking bekerja per **byte**, bukan karakter Unicode. Gunakan teks ASCII agar tidak memotong karakter UTF-8. Parameter `mask` tidak boleh kosong (akan melempar exception) dan hanya **byte pertama** yang dipakai.
{% endhint %}

### `$GT.mask_prefix`

Mengganti `n` byte **awal**.

```js
$GT.mask_prefix(value, mask, n)
```

| Parameter | Keterangan |
|---|---|
| `value` | String |
| `mask` | String tidak kosong, byte pertama dipakai |
| `n` | Integer, jumlah byte |

Jika `n <= 0` atau `n >=` panjang byte string, nilai asli dikembalikan **tanpa masking**.

```js
$GT.mask_prefix('123456', '*', 2);  // **3456
$GT.mask_prefix('123456', '#', 3);  // ###456
$GT.mask_prefix('123456', '*', 0);  // "123456"
$GT.mask_prefix('123456', '*', 6);  // "123456"
$GT.mask_prefix('123456', 'XY', 2); // XX3456 (hanya "X" dipakai)
```

### `$GT.mask_suffix`

Mengganti `n` byte **akhir**. Parameter dan aturan sama dengan `mask_prefix`.

```js
$GT.mask_suffix(value, mask, n)
```

```js
$GT.mask_suffix('123456', '*', 2);  // 1234**
$GT.mask_suffix('123456', '#', 3);  // 123###
$GT.mask_suffix('123456', '*', 0);  // "123456"
$GT.mask_suffix('123456', '*', 6);  // "123456"
$GT.mask_suffix('123456', 'XY', 2); // 1234XX
```

### `$GT.mask`

Mengganti rentang byte tertentu.

```js
$GT.mask(value, mask, start, end)
```

| Parameter | Keterangan |
|---|---|
| `start`, `end` | Indeks byte berbasis 0. **Kedua batas inklusif** |

Aturan batas:

* Indeks negatif dijadikan `0`.
* `start`/`end` terbalik ditukar.
* `start` di luar panjang mengembalikan string asli.

{% hint style="danger" %}
**Kejanggalan implementasi:** `end >=` panjang byte dapat memicu exception indeks di luar batas, karena loop bersifat inklusif. Batasi `end` paling besar **panjang byte − 1**.
{% endhint %}

```js
$GT.mask('123456', '*', 1, 3);   // "1***56"
$GT.mask('123456', '#', 3, 1);   // "1###56"
$GT.mask('123456', '*', -2, 1);  // "**3456"
$GT.mask('123456', '*', 0, 5);   // "******"
try { $GT.mask('123456', '*', 0, 6); }
catch (err) { $GT.log(String(err)); } // batas tidak valid
```

## `$GT.xml2json`

Mengubah string XML menjadi nilai JavaScript.

```js
$GT.xml2json(xml)
```

| Parameter | Keterangan |
|---|---|
| `xml` | String XML |

**Hasil:** object/array/string sesuai struktur XML. **Bukan** string JSON.

{% hint style="info" %}
* Elemen berulang dapat menjadi array.
* Isi campuran dapat memakai kunci `#content`.
* XML tidak valid melempar exception.
* Bentuk detail mengikuti decoder `goxml2json`.
{% endhint %}

```js
const a = $GT.xml2json('<root><id>7</id></root>');
const b = $GT.xml2json('<root><item>A</item><item>B</item></root>');
const text = JSON.stringify(b); // jika perlu string JSON
try { $GT.xml2json('<root>'); }
catch (err) { $GT.log(String(err)); }
```

## `$GT.transform`

Memetakan ulang struktur JSON dengan spesifikasi sederhana.

```js
$GT.transform(jsonText, spec)
```

| Parameter | Tipe | Keterangan |
|---|---|---|
| `jsonText` | string | **String JSON**, bukan object langsung |
| `spec` | `Array<object>` | Tiap object berisi pasangan `namaOutput: 'path.input'` |

**Hasil:** nilai JavaScript hasil transformasi.

**Cara kerja**

* Setiap object `spec` adalah **satu tahap**. Tahap berikutnya membaca hasil tahap sebelumnya.
* Input array diproses **per elemen**.
* Path yang tidak ditemukan menjadi **literal string** path itu, dengan awalan `!` dihapus. Ini berguna untuk nilai konstan, misalnya `'!new'`.
* `spec` kosong (`[]`) mempertahankan nilai JSON apa adanya.
* JSON tidak valid melempar exception.

```js
$GT.transform('{"user":{"id":7}}', [{id: 'user.id'}]);      // {id:7}
$GT.transform('[{"id":1},{"id":2}]', [{code: 'id'}]);       // [{code:1},{code:2}]
$GT.transform('{"id":7}', [{temp: 'id'}, {code: 'temp'}]);  // dua tahap
$GT.transform('{}', [{status: '!new'}]);                    // {status:"new"}
$GT.transform('{"id":7}', []);                              // {id:7}
$GT.transform(JSON.stringify({id: 7}), [{code: 'id'}]);
```
