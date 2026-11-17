---
description: "Request validation, request transformer, response transformer, dan log transformer."
---

# Filter

Filter dipasang pada **endpoint** untuk memeriksa atau mengubah data yang lewat. Ada empat jenis.

| Filter | Fungsi | Format |
|---|---|---|
| **Request validation** | Memastikan request (body, headers, queries) sesuai aturan | JSON Schema |
| **Request transformer** | Mengubah body request agar sesuai kebutuhan | GJSON |
| **Response transformer** | Mengubah body response agar sesuai kebutuhan | GJSON |
| **Log transformer** | Mengubah isi log, baik request maupun response | GJSON |

{% hint style="warning" %}
Endpoint **database** dan **JavaScript** tidak memiliki response transformer.
{% endhint %}

## Request validation

Berfungsi menangani request (body, headers, queries) agar sesuai persyaratan yang Anda tentukan. Formatnya mengikuti **JSON Schema**.

**Contoh:** wajib mengirim query `startdate` dan `enddate` berformat `YYYY-MM-DD`, masing-masing satu nilai.

```json
{
  "queries": {
    "schema": {
      "type": "object",
      "required": ["startdate", "enddate"],
      "properties": {
        "startdate": {
          "type": "array",
          "minItems": 1,
          "maxItems": 1,
          "items": {
            "type": "string",
            "pattern": "^\\d{4}-\\d{2}-\\d{2}$"
          }
        },
        "enddate": {
          "type": "array",
          "minItems": 1,
          "maxItems": 1,
          "items": {
            "type": "string",
            "pattern": "^\\d{4}-\\d{2}-\\d{2}$"
          }
        }
      }
    },
    "message": "Invalid Parameters",
    "code": 400
  }
}
```

Cara membacanya:

* Kunci `queries` menandakan bagian request yang diperiksa.
* `schema` berisi aturan JSON Schema. Nilai query berbentuk **array**, jadi tipenya `array` dengan `items` bertipe string.
* `required` menyebut field yang wajib ada.
* `message` dan `code` adalah response yang dikirim bila request tidak lolos (di sini `400 Invalid Parameters`).

## Request transformer

Mengubah **body request** sebelum diteruskan. Scriptnya mengikuti format **GJSON**: sebuah array berisi object, tempat tiap pasangan berbentuk `"NamaBaru": "path.lama"`.

```json
[
  {
    "Name": "name",
    "Age": "age",
    "Role": "role",
    "additional_field": "!Optional"
  }
]
```

Artinya:

| Pasangan | Hasil |
|---|---|
| `"Name": "name"` | Field `Name` di hasil diisi dari field `name` pada request |
| `"Age": "age"` | Field `Age` diisi dari `age` |
| `"Role": "role"` | Field `Role` diisi dari `role` |
| `"additional_field": "!Optional"` | Awalan `!` berarti **nilai tetap**: field diisi teks `Optional` |

{% hint style="info" %}
Pola `nama: path` dan awalan `!` ini mirip dengan fungsi [`$GT.transform`](connection/javascript/methods/panduan/ubah-dan-samarkan-data.md#memetakan-ulang-struktur-json-transform) di script JavaScript. Tafsir ini kami simpulkan dari contoh pada materi, jadi uji dulu dengan data Anda.
{% endhint %}

## Response transformer

Mengubah **body response** sebelum dikirim ke konsumen. Scriptnya mengikuti format **GJSON**, sama seperti request transformer. Hanya tersedia pada endpoint **HTTP** (tidak pada database dan JavaScript).

## Log transformer

Mengubah isi **log**, baik log request maupun response (misalnya untuk menyembunyikan data sensitif). Scriptnya mengikuti format **GJSON**.
