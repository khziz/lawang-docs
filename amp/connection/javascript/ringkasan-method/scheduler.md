# Scheduler & sinkronisasi

{% hint style="info" %}
Method di halaman ini dipakai pada **script job di DSP**. Cara membuat server scheduler, jadwal, dan job ada di [Membuat job di DSP](../../../../dsp/sinkronisasi.md).
{% endhint %}

{% hint style="success" %}
Baru belajar? Baca dulu [panduan langkah demi langkah](../buat-endpoint-api/job-scheduler.md). Halaman ini adalah **referensi teknis** untuk dicari ulang.
{% endhint %}

**Mode:** scheduler saja. Method di halaman ini tidak tersedia di gateway.

## `$GT.run`

Menjalankan satu job anak dan **menunggu sampai selesai**.

```js
$GT.run(jobCode, exception?)
```

| Parameter   | Tipe    | Keterangan     |
| ----------- | ------- | -------------- |
| `jobCode`   | string  | Kode job       |
| `exception` | boolean | Default `true` |

**Hasil:** `null` jika sukses. Jika `exception=false` dan eksekusi gagal, hasilnya **string error**.

{% hint style="info" %}
* `exception=false` hanya menekan error **eksekusi** job. Error awal seperti job tidak ditemukan atau argumen kurang tetap melempar exception.
* Kedalaman nested maksimum **8**.
* Job anak mengikuti context dan batas kapasitas scheduler.
{% endhint %}

```js
$GT.run('import-items');
$GT.run('import-items', true);  // throw jika eksekusi gagal
const error = $GT.run('import-items', false);
if (error !== null) { $GT.log(error); }
try { $GT.run('job-tidak-ada', false); }
catch (err) { $GT.log(String(err)); }  // lookup tetap throw
```

## `$GT.parallel`

Menjalankan beberapa job anak **secara paralel**, lalu menunggu semuanya selesai.

```js
$GT.parallel(jobCodes, exception?)
```

| Parameter   | Tipe       | Keterangan      |
| ----------- | ---------- | --------------- |
| `jobCodes`  | `string[]` | Daftar kode job |
| `exception` | boolean    | Default `true`  |

**Hasil:** `null` jika sukses, string error jika `exception=false` dan gagal.

{% hint style="info" %}
* Semua kode divalidasi **sebelum** dijalankan.
* Jumlah job tidak boleh melebihi `maxConcurrentNestedJobs` di konfigurasi.
* Error pada satu child membatalkan context seluruh kelompok.
* Ini **bukan** `Promise.all` dan tidak mengembalikan hasil tiap job.
{% endhint %}

```js
$GT.parallel(['import-a', 'import-b']);
$GT.parallel(['import-a', 'import-b'], true);
const error = $GT.parallel(['import-a', 'import-b'], false);
if (error !== null) { $GT.log(error); }
$GT.parallel([]); // null, tidak ada child
```

## `$GT.sync`

Menyinkronkan data dari query database sumber ke tabel database tujuan.

```js
$GT.sync(source, target, options?)
```

**`source`**

| Field        | Wajib | Keterangan                 |
| ------------ | ----- | -------------------------- |
| `connection` | ya    | Nama connection            |
| `query`      | ya    | Query SELECT               |
| `parameters` | tidak | `any[]` parameter query    |
| `agent`      | tidak | Nama agent (untuk up-sync) |

**`target`**

| Field            | Wajib | Keterangan                              |
| ---------------- | ----- | --------------------------------------- |
| `connection`     | ya    | Nama connection                         |
| `table`          | ya    | Tabel tujuan                            |
| `identityFields` | tidak | `string[]` kolom identitas untuk upsert |
| `agent`          | tidak | Nama agent (untuk down-sync)            |

**`options`**

| Field       | Default            | Batas          |
| ----------- | ------------------ | -------------- |
| `batchSize` | 1000               | 1 sampai 10000 |
| `worker`    | jumlah CPU runtime | 1 sampai 256   |

**Hasil:** `null` setelah sinkronisasi selesai. Gagal melempar exception.

**Tiga arah sinkronisasi**

| Arah                               | Cara               |
| ---------------------------------- | ------------------ |
| Scheduler ke scheduler             | Tanpa `agent`      |
| Agent ke scheduler (**up-sync**)   | Isi `source.agent` |
| Scheduler ke agent (**down-sync**) | Isi `target.agent` |

{% hint style="warning" %}
Gunakan `agent` di **satu sisi saja**. Mode agent di kedua sisi bukan kontrak relay. `identityFields` menentukan identitas untuk upsert, sedangkan validasi dan nama constraint mengikuti database.
{% endhint %}

```js
// Backend database scheduler ke backend scheduler
$GT.sync({connection: 'db_a', query: 'SELECT id, name FROM items'},
         {connection: 'db_b', table: 'items', identityFields: ['id']});

// Dengan parameter query dan opsi parsial
$GT.sync({connection: 'db_a',
          query: 'SELECT id, name FROM items WHERE id > ?', parameters: [10]},
         {connection: 'db_b', table: 'items', identityFields: ['id']},
         {batchSize: 500, worker: 4});

// Agent ke scheduler (up-sync)
$GT.sync({agent: 'branch', connection: 'local_db',
          query: 'SELECT id FROM items'},
         {connection: 'db_b', table: 'items', identityFields: ['id']},
         {worker: 2});

// Scheduler ke agent (down-sync)
$GT.sync({connection: 'db_a', query: 'SELECT id FROM items'},
         {agent: 'branch', connection: 'local_db', table: 'items',
          identityFields: ['id']}, {batchSize: 100});
```

## `$GT.rfc_sync`

Membaca tabel keluaran SAP lalu menyinkronkannya ke database scheduler.

```js
$GT.rfc_sync(source, target, options?)
```

**`source`**

| Field        | Keterangan                                                              |
| ------------ | ----------------------------------------------------------------------- |
| `connection` | Nama connection SAP                                                     |
| `function`   | Nama fungsi RFC                                                         |
| `parameters` | object parameter. **Selalu isi**, pakai `{}` jika kosong                |
| `output`     | `{key, fields}`. `key` **wajib**. `fields: []` memilih kolom dari hasil |

**`target`:** `{connection, table, identityFields?}`. **`options`:** `{batchSize?, worker?}` dengan default dan batas sama seperti `sync`.

**Hasil:** `null` setelah sinkronisasi selesai.

{% hint style="danger" %}
* Jika `parameters` dihilangkan, parser source berhenti sebelum membaca `output`. **Selalu berikan** `parameters: {}` bila kosong.
* `target.agent` **tidak** dipakai untuk routing oleh `rfc_sync`.
{% endhint %}

```js
$GT.rfc_sync({connection: 'sap', function: 'Z_READ_ITEMS',
              parameters: {}, output: {key: 'ITEMS', fields: []}},
             {connection: 'db_b', table: 'items', identityFields: ['ID']});

$GT.rfc_sync({connection: 'sap', function: 'Z_READ_ITEMS',
              parameters: {ACTIVE: 'X'},
              output: {key: 'ITEMS', fields: ['ID', 'NAME']}},
             {connection: 'db_b', table: 'items', identityFields: ['ID']},
             {batchSize: 500, worker: 4});
```
