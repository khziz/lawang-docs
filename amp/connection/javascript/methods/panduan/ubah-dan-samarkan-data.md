# Mengubah dan menyamarkan data

**Mode:** gateway dan scheduler. **Anda akan belajar:** menyamarkan data sensitif, membaca XML, dan merapikan struktur JSON.

## Menyamarkan data (masking)

Cocok untuk nomor kartu, telepon, atau email sebelum ditampilkan atau dicatat.

| Method | Menyamarkan | Contoh |
|---|---|---|
| `mask_prefix(teks, tanda, n)` | `n` karakter **awal** | `mask_prefix('123456', '*', 2)` → `**3456` |
| `mask_suffix(teks, tanda, n)` | `n` karakter **akhir** | `mask_suffix('123456', '*', 2)` → `1234**` |
| `mask(teks, tanda, awal, akhir)` | Rentang tertentu | `mask('123456', '*', 1, 3)` → `1***56` |

Hal yang perlu diperhatikan:

* Bekerja per **byte**. Gunakan teks ASCII agar karakter tidak terpotong.
* `tanda` tidak boleh kosong. Hanya karakter pertamanya yang dipakai.
* `mask_prefix`/`mask_suffix`: jika `n` ≤ 0 atau ≥ panjang teks, teks **tidak disamarkan**.
* `mask`: `awal` dan `akhir` sama-sama **ikut disamarkan** (inklusif), mulai dari 0. Nilai `akhir` maksimum **panjang − 1**.

```js
$GT.mask('123456', '*', 0, 5);   // "******"
$GT.mask('123456', '#', 3, 1);   // "1###56" (urutan terbalik ditukar otomatis)
```

## Mengubah XML menjadi object

```js
const a = $GT.xml2json('<root><id>7</id></root>');
const b = $GT.xml2json('<root><item>A</item><item>B</item></root>');
```

* Hasilnya **object/array JavaScript**, bukan teks JSON.
* Elemen berulang bisa menjadi array.
* XML rusak melempar error.
* Perlu teks JSON? Pakai `JSON.stringify(hasil)`.

## Memetakan ulang struktur JSON (`transform`)

`transform` mengubah bentuk JSON memakai daftar aturan `namaBaru: 'path.lama'`. Input harus **teks JSON**.

```js
$GT.transform('{"user":{"id":7}}', [{id: 'user.id'}]);
// {id: 7}
```

Cara kerjanya:

1. Setiap object di array aturan adalah **satu tahap**. Tahap berikutnya membaca hasil tahap sebelumnya.
2. Jika inputnya array, aturan diterapkan **per elemen**.
3. Jika path tidak ditemukan, hasilnya teks path itu sendiri tanpa awalan `!`. Cara ini dipakai untuk **nilai tetap**.
4. Array aturan kosong `[]` = data tidak diubah.

```js
$GT.transform('[{"id":1},{"id":2}]', [{code: 'id'}]);        // ganti nama field
$GT.transform('{"id":7}', [{temp: 'id'}, {code: 'temp'}]);   // dua tahap
$GT.transform('{}', [{status: '!new'}]);                     // {status: "new"}
$GT.transform(JSON.stringify({id: 7}), [{code: 'id'}]);      // dari object
```

**Referensi:** [Masking & transformasi](../referensi/masking-transformasi.md)
