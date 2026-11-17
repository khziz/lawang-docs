# Utilitas & logging

## `$GT.log`

Menulis log terstruktur (menyertakan `timestamp` dan `request_id`). **Mode:** gateway dan scheduler.

```js
$GT.log(value)
```

| Parameter | Keterangan                                                                 |
| --------- | -------------------------------------------------------------------------- |
| `value`   | **Tepat satu** nilai: string, number, boolean, object, array, null, BigInt |

**Hasil:** `boolean`. `true` jika seluruh log berhasil ditulis, `false` jika jumlah argumen salah atau penulisan gagal.

{% hint style="info" %}
* BigInt dicatat sebagai string.
* Referensi melingkar menjadi `[Circular]`.
* Kedalaman lebih dari 64 menjadi `[MaxDepth]`.
* Tidak sama dengan `console.log` yang menerima banyak argumen.
{% endhint %}

```js
$GT.log('mulai');
$GT.log({step: 'import', count: 10});
$GT.log([1, 2, 3]);
$GT.log(123n);            // dicatat sebagai string
$GT.log();                // false
$GT.log('a', 'b');        // false
const circular = {}; circular.self = circular;
$GT.log(circular);        // self: "[Circular]"
```

## `$GT.uuid`

Membuat UUID versi 4. **Mode:** gateway dan scheduler.

```js
$GT.uuid()
```

Tidak membutuhkan parameter (argumen tambahan diabaikan). **Hasil:** string UUID v4 baru di setiap pemanggilan.

```js
const id = $GT.uuid();
const event = {id: $GT.uuid(), type: 'created'};
```
