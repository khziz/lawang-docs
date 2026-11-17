# Data yang tersedia di skrip

Selain fungsi, `$GT` juga membawa **data** tentang request atau job yang sedang berjalan. Ini adalah **properti** (tanpa tanda kurung), bukan fungsi.

{% hint style="info" %}
Banyak properti berisi **teks JSON**. Ubah dulu dengan `JSON.parse(...)` sebelum dipakai sebagai object.
{% endhint %}

## Di gateway

### Isi request

| Properti      | Isi                                   | Perlu `JSON.parse`?  |
| ------------- | ------------------------------------- | -------------------- |
| `$GT.payload` | Body request (teks)                   | Ya, jika isinya JSON |
| `$GT.queries` | Query string, mis. `?page=2`          | Ya                   |
| `$GT.params`  | Parameter di path, mis. `/items/{id}` | Ya                   |
| `$GT.headers` | Header request                        | Ya                   |
| `$GT.files`   | File yang diunggah (Base64)           | Ya                   |

`queries`, `params`, dan `headers` hanya ada bila endpoint dikonfigurasi untuk menerimanya. Karena itu selalu periksa dulu:

```js
const query = $GT.queries ? JSON.parse($GT.queries) : {};
```

{% hint style="warning" %}
Nilai di `queries` dan `headers` berupa **array**. Ambil elemen pertama: `query.page[0]`.
{% endhint %}

### Identitas request dan pemanggil

| Properti                                      | Isi                                           |
| --------------------------------------------- | --------------------------------------------- |
| `$GT.request_id`                              | ID request                                    |
| `$GT.method`                                  | Metode HTTP (GET, POST, ...)                  |
| `$GT.uri`                                     | URI request                                   |
| `$GT.client_ip`                               | IP pemanggil                                  |
| `$GT.client_id`, `client_code`, `client_name` | Data client, **bila** credential punya client |
| `$GT.client_group_code`, `client_group_name`  | Data grup client, bila ada                    |

### Contoh lengkap

```js
const payload = $GT.payload ? JSON.parse($GT.payload) : {};
const query   = $GT.queries ? JSON.parse($GT.queries) : {};
const page    = query.page ? query.page[0] : '1';
const files   = JSON.parse($GT.files);   // default "{}"
```

## Di scheduler

| Properti       | Isi         |
| -------------- | ----------- |
| `$GT.run_id`   | ID eksekusi |
| `$GT.job_code` | Kode job    |
| `$GT.job_name` | Nama job    |

```js
$GT.log({run: $GT.run_id, job: $GT.job_code});
```
