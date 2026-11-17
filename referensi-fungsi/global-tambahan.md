# Global tambahan

Tiga helper global yang tersedia tanpa awalan `$GT`.

## `atob`

Mendekode string Base64 standar. **Mode:** gateway dan scheduler.

```js
atob(value?)
```

| Parameter | Keterangan                                                                                    |
| --------- | --------------------------------------------------------------------------------------------- |
| `value`   | Nilai yang dikonversi menjadi string Base64 standar. Tanpa argumen menghasilkan string kosong |

**Hasil:** string hasil decode. String kosong jika Base64 tidak valid.

{% hint style="info" %}
* Memakai Base64 standar **dengan padding**.
* Tidak melempar exception untuk input yang gagal didekode.
* Hasilnya string dari byte Go, jangan mengasumsikan semua semantik _binary string_ browser.
{% endhint %}

```js
atob('SGFsbw==');      // "Halo"
atob(btoa('Lawang'));  // "Lawang"
atob('%%%');           // ""
atob();                // ""
```

## `btoa`

Mengodekan string menjadi Base64. **Mode:** gateway dan scheduler.

```js
btoa(value?)
```

| Parameter | Keterangan                                                                           |
| --------- | ------------------------------------------------------------------------------------ |
| `value`   | Dikonversi menjadi string, lalu byte UTF-8. Tanpa argumen menghasilkan string kosong |

**Hasil:** string Base64 standar dengan padding.

{% hint style="info" %}
Berbeda dari `btoa` browser yang dibatasi Latin-1, versi ini mengodekan byte **UTF-8**.
{% endhint %}

```js
btoa('Halo');  // "SGFsbw=="
btoa(123);     // "MTIz"
btoa('');      // ""
btoa();        // ""
```

## `console.log`

Menulis log sederhana. **Mode:** scheduler, serta gateway **non-production**.

```js
console.log(...values)
```

| Parameter | Keterangan                                                                         |
| --------- | ---------------------------------------------------------------------------------- |
| `values`  | Nol atau lebih nilai. Masing-masing dikonversi ke string dan dipisahkan satu spasi |

**Hasil:** `undefined`.

{% hint style="warning" %}
* Object **tidak** di-`JSON.stringify` otomatis.
* **Gateway:** hanya ada di non-production. Output log dapat ditambahkan ke body respons (field `log` untuk objek JSON, atau teks `-> LOG:`).
* **Scheduler:** tersedia juga di production dan menulis ke output log.
{% endhint %}

```js
console.log('mulai');
console.log('jumlah', 3, true);          // "jumlah 3 true"
console.log(JSON.stringify({id: 7}));
console.log();                           // tidak menulis newline
```

Untuk log terstruktur, gunakan [`$GT.log`](utilitas-logging.md).
