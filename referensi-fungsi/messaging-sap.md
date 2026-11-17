# Messaging & SAP

{% hint style="success" %}
Baru belajar? Baca dulu [panduan langkah demi langkah](../panduan-per-tugas/kirim-pesan-dan-sap.md). Halaman ini adalah **referensi teknis** untuk dicari ulang.
{% endhint %}

**Mode:** gateway dan scheduler, untuk semua fungsi di halaman ini.

## `$GT.publish`

Mengirim pesan ke backend event-driven.

```js
$GT.publish(source, topic, payload)
```

| Parameter | Tipe           | Keterangan                     |
| --------- | -------------- | ------------------------------ |
| `source`  | string         | Nama backend event-driven      |
| `topic`   | string         | Topic tujuan                   |
| `payload` | object / array | Harus dapat diserialisasi JSON |

**Hasil:** `null` saat sukses.

{% hint style="warning" %}
* **String JSON langsung ditolak** dengan pesan `payload must be an object`. Parse dulu dengan `JSON.parse`.
* Pesan diberi UUID dan memakai context request/job.
* Error backend atau publish melempar exception.
{% endhint %}

```js
$GT.publish('events', 'item.created', {id: 7});
$GT.publish('events', 'items.batch', [{id: 7}, {id: 8}]);
$GT.publish('events', 'item.created', JSON.parse('{"id":7}'));
```

## `$GT.rfc`

Memanggil fungsi RFC SAP.

```js
$GT.rfc(source, functionName, parameters, output?, timeout?)
```

| Parameter      | Tipe                          | Keterangan                                  |
| -------------- | ----------------------------- | ------------------------------------------- |
| `source`       | string                        | Nama backend SAP                            |
| `functionName` | string                        | Nama fungsi RFC                             |
| `parameters`   | object                        | Parameter RFC. Boleh `{}`                   |
| `output`       | `{key, fields}` / string JSON | Filter hasil (lihat di bawah)               |
| `timeout`      | integer                       | Detik, mengikuti kebijakan `RequestTimeout` |

**Format `output`**

| Field    | Keterangan                                                                        |
| -------- | --------------------------------------------------------------------------------- |
| `key`    | Nama tabel keluaran yang diambil. Jika kosong, hasil RFC lengkap dikembalikan     |
| `fields` | `string[]` kolom yang dipilih. Jika kosong, kolom diambil dari **record pertama** |

**Hasil**

* `key` kosong: object hasil RFC lengkap.
* `key` diisi: array record terpilih (**maksimum 1.000 baris**).

{% hint style="warning" %}
- Untuk timeout eksplisit **tanpa filter**, isi slot output dengan `{}`.
- Integer di slot output **tidak aman**: parser tetap mencoba JSON-decode `output`, sehingga bisa gagal.
- Error RFC dapat berupa object terstruktur (lihat [aturan runtime](../konsep-dasar/error-dan-timeout.md)).
{% endhint %}

```js
$GT.rfc('sap', 'STFC_CONNECTION', {REQUTEXT: 'Halo'});
$GT.rfc('sap', 'STFC_CONNECTION', {REQUTEXT: 'Halo'}, {}, 10);
$GT.rfc('sap', 'Z_READ_ITEMS', {LIMIT: 50},
  {key: 'ITEMS', fields: ['ID', 'NAME']}, 30);
$GT.rfc('sap', 'Z_READ_ITEMS', {}, {key: 'ITEMS', fields: []});
$GT.rfc('sap', 'Z_READ_ITEMS', {},
  '{"key":"ITEMS","fields":["ID"]}', 10);
```
