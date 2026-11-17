# Mengirim pesan dan memanggil SAP

**Mode:** gateway dan scheduler.

## Mengirim pesan ke event backend (`publish`)

Dipakai untuk memberi tahu sistem lain bahwa sesuatu terjadi, misalnya "item dibuat".

```js
$GT.publish('events', 'item.created', {id: 7});
$GT.publish('events', 'items.batch', [{id: 7}, {id: 8}]);
```

* Argumen: nama backend, **topic**, lalu payload.
* Payload harus **object atau array**. String JSON ditolak.

```js
const data = JSON.parse($GT.payload);              // ubah teks jadi object dulu
$GT.publish('events', 'item.created', data);
```

* Sukses mengembalikan `null`. Gagal melempar error.

## Memanggil method SAP (`rfc`)

```js
$GT.rfc('sap', 'STFC_CONNECTION', {REQUTEXT: 'Halo'});
```

Urutan argumen: nama backend SAP, nama fungsi RFC, parameter (object, boleh `{}`), lalu opsional `output` dan `timeout`.

### Mengambil sebagian hasil

Hasil RFC bisa besar. Pakai `output` untuk memilih **satu tabel** dan **kolom** tertentu:

```js
const items = $GT.rfc('sap', 'Z_READ_ITEMS', {LIMIT: 50},
  {key: 'ITEMS', fields: ['ID', 'NAME']}, 30);
```

| Field `output` | Arti |
|---|---|
| `key` | Nama tabel keluaran yang diambil. Jika kosong, **seluruh hasil** dikembalikan |
| `fields` | Kolom yang dipilih. Jika `[]`, kolom diambil dari record pertama |

Hasil dengan `key` berupa **array record** (maksimum 1.000 baris).

### Memasang timeout tanpa filter

Isi slot `output` dengan `{}`:

```js
$GT.rfc('sap', 'STFC_CONNECTION', {REQUTEXT: 'Halo'}, {}, 10);
```

{% hint style="warning" %}
Jangan menaruh angka di slot `output`. Parser tetap mencoba membacanya sebagai JSON dan bisa gagal.
{% endhint %}

### Menangani error SAP

Error RFC bisa berupa object. Lihat [Error dan timeout](../konsep/error-dan-timeout.md).

```js
try {
  $GT.rfc('sap', 'Z_READ_ITEMS', {}, {key: 'ITEMS', fields: []});
} catch (err) {
  $GT.log(typeof err === 'object' ? err : String(err));
}
```

**Referensi:** [Messaging & SAP](../referensi/messaging-sap.md)
