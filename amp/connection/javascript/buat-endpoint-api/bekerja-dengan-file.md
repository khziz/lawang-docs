# Bekerja dengan file, CSV, dan Excel

**Mode:** gateway dan scheduler. **Anda akan belajar:** membaca, menyimpan, dan mengimpor file.

Semua method memakai **nama backend file** (misalnya `'files'`). `path` selalu relatif terhadap lokasi backend. Method file tidak punya parameter timeout.

## Membaca isi file

```js
const text = $GT.load_file('files', 'input/data.txt');           // teks
const obj  = JSON.parse($GT.load_file('files', 'input/data.json'));
```

Hasilnya teks biasa, **bukan** Base64. Bila perlu Base64: `btoa(text)`.

## Mengecek file

`check_file` mengembalikan **ukuran dalam byte**, bukan benar/salah.

```js
const size = $GT.check_file('files', 'input/data.csv');
if (size === 0) { $GT.log('file kosong'); }
```

File yang tidak ada dapat melempar error. Gunakan `try/catch` untuk mengeceknya:

```js
let ada = true;
try { $GT.check_file('files', 'input/data.csv'); }
catch (err) { ada = false; }
```

## Membuat folder dan menyimpan file

```js
$GT.mkdir('files', 'output/2026/10');
$GT.store_file('files', btoa('Halo'), 'output/hello.txt');
```

{% hint style="danger" %}
Urutan argumen `store_file`: **backend, isi Base64, path**. Isi dulu, baru path. File lama di path yang sama **ditimpa**.
{% endhint %}

### Menyimpan file yang diunggah

Di gateway, file unggahan ada di `$GT.files` (sudah dalam Base64):

```js
const uploaded = JSON.parse($GT.files);
if (uploaded.document) {
  $GT.store_file('files', uploaded.document.content, 'output/document.bin');
}
```

Setiap field berisi `name`, `ext`, dan `content`.

## Membaca CSV

```js
const rows = $GT.read_csv('files', 'input/data.csv');          // koma
const semi = $GT.read_csv('files', 'input/semicolon.csv', ';'); // titik koma
const tab  = $GT.read_csv('files', 'input/tab.tsv', '\t');      // tab
```

* Hasilnya array 2 dimensi berisi **teks**: `[['id','nama'], ['1','A']]`.
* **Baris header ikut** sebagai baris pertama. Lewati dengan `.slice(1)`.
* File tanpa isi menghasilkan `null`, jadi lindungi dengan `(rows || [])`.
* Angka dan boolean tidak diubah otomatis, gunakan `Number(...)` bila perlu.

## Membaca Excel

```js
const rows = $GT.read_excel('files', 'input/data.xlsx', 0);   // sheet pertama
const second = $GT.read_excel('files', 'input/data.xlsx', 1); // sheet kedua
```

Argumen ketiga adalah **nomor sheet** mulai 0, bukan nama. Baris kosong bisa menjadi `null`, jadi saring dulu:

```js
const values = rows.filter(row => row !== null);
```

## Contoh: impor CSV ke database

```js
const rows = ($GT.read_csv('files', 'input/items.csv') || []).slice(1); // buang header
const records = rows.map(r => ({id: Number(r[0]), name: r[1]}));

$GT.upsert('db_main', 'items', ['id'], records, 30);
$GT.log({diimpor: records.length});
```

**Referensi:** [File, CSV, & Excel](../ringkasan-method/file.md)
