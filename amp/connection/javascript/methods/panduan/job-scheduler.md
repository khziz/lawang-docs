# Mengatur job di scheduler

{% hint style="info" %}
Method di halaman ini dipakai pada **script job di DSP**. Cara membuat server scheduler, jadwal, dan job ada di [Membuat job di DSP](../../../../../dsp/membuat-job.md).
{% endhint %}

**Mode:** scheduler saja. **Anda akan belajar:** menjalankan job lain, menjalankan beberapa job bersamaan, dan menyinkronkan data.

## Pilih method yang tepat

| Kebutuhan | Method |
|---|---|
| Jalankan **satu** job lalu tunggu | `$GT.run` |
| Jalankan **beberapa** job bersamaan lalu tunggu semuanya | `$GT.parallel` |
| Salin data database ke database lain | `$GT.sync` |
| Salin tabel SAP ke database | `$GT.rfc_sync` |

## Menjalankan satu job (`run`)

```js
$GT.run('import-items');                       // gagal = melempar error
const error = $GT.run('import-items', false);  // gagal = mengembalikan teks error
if (error !== null) { $GT.log(error); }
```

* Menunggu job anak selesai.
* Kedalaman bersarang maksimum **8** tingkat.
* `false` hanya menekan error **eksekusi**. Job yang tidak ditemukan tetap melempar error.

## Menjalankan beberapa job bersamaan (`parallel`)

```js
$GT.parallel(['import-a', 'import-b']);
const error = $GT.parallel(['import-a', 'import-b'], false);
```

* Semua kode job diperiksa dulu sebelum dijalankan.
* Jumlah job tidak boleh melebihi `maxConcurrentNestedJobs`.
* Jika satu job gagal, kelompok dibatalkan.
* Hasilnya hanya `null` atau teks error. Hasil tiap job **tidak** dikembalikan.

## Menyusun alur

Jalankan langkah paralel dulu, lalu langkah yang bergantung padanya:

```js
$GT.log({run: $GT.run_id, step: 'start'});
$GT.parallel(['import-a', 'import-b']);
const error = $GT.run('build-summary', false);
if (error !== null) { throw error; }
$GT.log({run: $GT.run_id, step: 'done'});
```

## Menyinkronkan database (`sync`)

Menyalin hasil query dari database sumber ke tabel di database tujuan.

```js
$GT.sync(
  {connection: 'db_a', query: 'SELECT id, name FROM items'},
  {connection: 'db_b', table: 'items', identityFields: ['id']}
);
```

* `identityFields` menentukan baris yang dianggap sama (untuk upsert).
* Dengan parameter dan opsi:

```js
$GT.sync(
  {connection: 'db_a', query: 'SELECT id, name FROM items WHERE id > ?', parameters: [10]},
  {connection: 'db_b', table: 'items', identityFields: ['id']},
  {batchSize: 500, worker: 4}
);
```

| Opsi | Bawaan | Batas |
|---|---|---|
| `batchSize` | 1000 | 1 sampai 10000 |
| `worker` | jumlah CPU | 1 sampai 256 |

### Tiga arah sinkronisasi

| Arah | Cara menulis |
|---|---|
| Scheduler → scheduler | Tanpa `agent` |
| Agent → scheduler (**up-sync**) | `source: {agent: 'branch', ...}` |
| Scheduler → agent (**down-sync**) | `target: {agent: 'branch', ...}` |

Pakai `agent` di **satu sisi saja**.

## Menyinkronkan tabel SAP (`rfc_sync`)

```js
$GT.rfc_sync(
  {connection: 'sap', function: 'Z_READ_ITEMS',
   parameters: {}, output: {key: 'ITEMS', fields: []}},
  {connection: 'db_b', table: 'items', identityFields: ['ID']}
);
```

{% hint style="danger" %}
Selalu isi `parameters`, pakai `{}` bila kosong. Jika dihilangkan, parser berhenti sebelum membaca `output`. Selain itu, `output.key` wajib diisi.
{% endhint %}

Opsi `batchSize` dan `worker` sama seperti `sync`. `target.agent` tidak dipakai oleh `rfc_sync`.

**Referensi:** [Scheduler & sinkronisasi](../referensi/scheduler.md)
