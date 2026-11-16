# Properti konteks `$GT`

Properti berikut adalah **data**, bukan fungsi. Ketersediaannya bergantung pada mode dan request.

{% hint style="info" %}
Properti bernilai string JSON (misalnya `queries`, `headers`) perlu di-`JSON.parse` dulu sebelum diakses sebagai object.
{% endhint %}

## Gateway

| Properti | Tipe | Isi |
|---|---|---|
| `payload` | string | Body request |
| `request_id` | string | ID request |
| `files` | string JSON | File upload per field: `name`, `ext`, `content` (Base64). Default `"{}"` |
| `client_ip` | string | IP klien |
| `uri` | string | RequestURI |
| `method` | string | Metode HTTP |
| `client_id`, `client_code`, `client_name` | string | Tersedia bila credential memiliki client |
| `client_group_code`, `client_group_name` | string | Tersedia bila credential ada |
| `queries` | string JSON | Map query ke array string. Hanya bila `endpoint.HasQuery` |
| `params` | string JSON | Map path parameter ke string. Hanya bila `endpoint.HasParam` |
| `headers` | string JSON | Map header ke array string. Hanya bila `endpoint.HasHeader` |

## Scheduler

| Properti | Tipe | Isi |
|---|---|---|
| `run_id` | string | ID eksekusi |
| `job_code` | string | Kode job |
| `job_name` | string | Nama job |

## Contoh

```js
const payload = $GT.payload ? JSON.parse($GT.payload) : {};
const query = $GT.queries ? JSON.parse($GT.queries) : {};
const firstPage = query.page ? query.page[0] : '1';
const files = JSON.parse($GT.files);

// Dalam scheduler:
$GT.log({run: $GT.run_id, job: $GT.job_code});
```

{% hint style="warning" %}
Nilai query berupa **array** string, jadi ambil elemen pertama dengan `query.page[0]`.
{% endhint %}
