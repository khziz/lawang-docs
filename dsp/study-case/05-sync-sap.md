# 5. Sinkronisasi tabel SAP

## Skenario

Data `ITEMS` ada di SAP dan bisa dibaca lewat fungsi RFC `Z_READ_ITEMS`. Anda ingin menyalinnya ke tabel `items` di database `db_b`.

## Komponen

| Komponen           | Nilai contoh                   |
| ------------------ | ------------------------------ |
| Connection SAP     | `sap`                          |
| Fungsi RFC         | `Z_READ_ITEMS`                 |
| Tabel keluaran SAP | `ITEMS`                        |
| Target             | Database `db_b`, tabel `items` |

## Script

```js
$GT.rfc_sync({
  connection: 'sap',
  function: 'Z_READ_ITEMS',
  parameters: {},
  output: {key: 'ITEMS', fields: []}
}, {
  connection: 'db_b',
  table: 'items',
  identityFields: ['ID']
});
```

| Bagian           | Fungsi                                                    |
| ---------------- | --------------------------------------------------------- |
| `function`       | Nama fungsi RFC                                           |
| `parameters`     | Parameter RFC. **Selalu isi**, pakai `{}` bila kosong     |
| `output.key`     | Tabel keluaran yang diambil. **Wajib diisi**              |
| `output.fields`  | Kolom yang dipilih. `[]` berarti kolom diambil dari hasil |
| `identityFields` | Kolom penentu baris yang sama untuk upsert                |

## Hasil

Baris tabel `ITEMS` dari SAP disalin ke `items` di `db_b`. Baris dengan `ID` yang sama diperbarui.

## Variasi

### Dengan parameter, kolom terpilih, dan opsi

```js
$GT.rfc_sync({
  connection: 'sap',
  function: 'Z_READ_ITEMS',
  parameters: {ACTIVE: 'X'},
  output: {key: 'ITEMS', fields: ['ID', 'NAME']}
}, {
  connection: 'db_b',
  table: 'items',
  identityFields: ['ID']
}, {batchSize: 500, worker: 4});
```

### Cek jumlah baris sesudah sinkronisasi

```js
const rows = $GT.query('db_b', 'SELECT COUNT(*) AS total FROM items', [], 5);
$GT.log({total: rows[0].total});
```

## Catatan

{% hint style="danger" %}
Jika `parameters` dihilangkan, parser berhenti sebelum membaca `output`. Selalu tulis `parameters: {}` bila kosong.
{% endhint %}

* `target.agent` **tidak** dipakai untuk routing oleh `rfc_sync`.
* Default dan batas `batchSize` dan `worker` sama seperti `$GT.sync`.
* Komunikasi ke SAP berjalan lewat adapter. Lihat [Connection SAP RFC](../../amp/connection/sap-rfc.md).

**Referensi:** [`$GT.rfc_sync`](../../amp/connection/javascript/ringkasan-method/scheduler.md)
