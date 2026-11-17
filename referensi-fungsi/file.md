# File, CSV, & Excel

{% hint style="success" %}
Baru belajar? Baca dulu [panduan langkah demi langkah](../panduan-per-tugas/bekerja-dengan-file.md). Halaman ini adalah **referensi teknis** untuk dicari ulang.
{% endhint %}

Fungsi untuk membaca dan menulis file lewat **backend file** yang sudah dikonfigurasi.

## Aturan umum

**Mode:** gateway dan scheduler, untuk semua fungsi di halaman ini.

* `source` adalah nama backend file yang sudah dikonfigurasi.
* `path` relatif terhadap lokasi backend.
* Timeout memakai `RequestTimeout`. **Tidak ada** parameter timeout per pemanggilan.
* Kesalahan backend, path, atau operasi **melempar exception**.

## `$GT.load_file`

Membaca isi file sebagai string.

```js
$GT.load_file(source, path)
```

**Hasil:** string isi file (**bukan** Base64).

```js
const text = $GT.load_file('files', 'input/data.txt');
const obj = JSON.parse($GT.load_file('files', 'input/data.json'));
const encoded = btoa($GT.load_file('files', 'input/data.txt'));
```

## `$GT.store_file`

Menyimpan file dari konten Base64.

```js
$GT.store_file(source, base64Content, path)
```

| Parameter       | Keterangan                    |
| --------------- | ----------------------------- |
| `source`        | Nama backend file             |
| `base64Content` | Base64 standar dengan padding |
| `path`          | Path tujuan                   |

**Hasil:** `null` saat sukses.

{% hint style="warning" %}
**Urutan penting:** isi Base64 dulu, baru path. Pada backend lokal, file lama **ditimpa** dan direktori induk dibuat otomatis.
{% endhint %}

```js
$GT.store_file('files', btoa('Halo'), 'output/hello.txt');
$GT.store_file('files', btoa(JSON.stringify({id: 7})),
  'output/data.json');

// Menyimpan file upload (gateway), jika field "document" tersedia:
const uploaded = JSON.parse($GT.files);
$GT.store_file('files', uploaded.document.content,
  'output/document.bin');
```

## `$GT.check_file`

Mengambil **ukuran** file.

```js
$GT.check_file(source, path)
```

**Hasil:** number, ukuran file dalam byte. **Bukan boolean.**

{% hint style="warning" %}
* File kosong berukuran `0`.
* File yang tidak ada dapat menghasilkan **exception** (termasuk backend lokal), bukan `false`. Bungkus dengan `try/catch` bila perlu.
{% endhint %}

```js
const size = $GT.check_file('files', 'input/data.csv');
if (size === 0) { $GT.log('file kosong'); }
try { $GT.check_file('files', 'input/missing.txt'); }
catch (err) { $GT.log(String(err)); }
```

## `$GT.mkdir`

Membuat direktori.

```js
$GT.mkdir(source, path)
```

**Hasil:** `null` saat sukses. Dukungan pembuatan direktori mengikuti backend.

```js
$GT.mkdir('files', 'output');
$GT.mkdir('files', 'output/2026/10');
```

## `$GT.read_csv`

Membaca CSV menjadi array dua dimensi.

```js
$GT.read_csv(source, path, delimiter?)
```

| Parameter   | Keterangan                                                                                  |
| ----------- | ------------------------------------------------------------------------------------------- |
| `delimiter` | String, default koma. Hanya **karakter pertama** dipakai. String kosong mempertahankan koma |

**Hasil:** `string[][]`. File tanpa record menghasilkan `null`.

{% hint style="info" %}
* Baris header tetap menjadi baris pertama, lewati sendiri bila perlu.
* Tidak mengubah tipe angka/boolean, semua nilai tetap string.
* Parser memeriksa jumlah field berdasarkan record pertama.
{% endhint %}

```js
const rows = $GT.read_csv('files', 'input/comma.csv');
$GT.read_csv('files', 'input/semicolon.csv', ';');
$GT.read_csv('files', 'input/tab.tsv', '\t');
$GT.read_csv('files', 'input/comma.csv', '');
const data = (rows || []).slice(1); // lewati header sendiri
```

## `$GT.read_excel`

Membaca satu sheet Excel.

```js
$GT.read_excel(source, path, sheetIndex?)
```

| Parameter    | Keterangan                                                        |
| ------------ | ----------------------------------------------------------------- |
| `sheetIndex` | Integer **indeks** sheet mulai 0, default 0. **Bukan** nama sheet |

**Hasil:** array baris berisi `string[]`. Baris nil dapat menjadi `null`.

{% hint style="info" %}
* Sel kosong di akhir baris dapat tidak disertakan oleh pembaca.
* Sheet atau file yang tidak valid menghasilkan exception.
{% endhint %}

```js
$GT.read_excel('files', 'input/data.xlsx');
$GT.read_excel('files', 'input/data.xlsx', 0);
$GT.read_excel('files', 'input/data.xlsx', 1); // sheet kedua
const rows = $GT.read_excel('files', 'input/data.xlsx', 0);
const values = rows.filter(row => row !== null);
```
