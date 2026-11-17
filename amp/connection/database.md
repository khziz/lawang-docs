---
description: "Menjadikan query SQL sebagai API lewat Connection Database."
---

# Connection database

Mengubah **query SQL** menjadi API. Konsumen memanggil endpoint HTTP, dan AMP menjalankan query di database Anda.

## 1. Connection database

| Field | Isi |
|---|---|
| **Name** | Bebas. Nama identitas sumber database |
| **Host** | Alamat database, misalnya `localhost:5432`. Parameter tambahan boleh ditulis, misalnya `localhost:5432?sslmode=disable` |
| **Driver** | `PostgreSQL`, `Oracle`, atau `MySQL` |
| **Database, username, password** | Sesuai credential database yang akan dikonfigurasi |

## 2. Endpoint database

| Field | Isi |
|---|---|
| **Connection type** | Pilih `SQL` |
| **Connection** | Pilih connection yang sudah dibuat (otomatis difilter ke SQL) |
| **API Path** | Path bebas, misalnya `/query` |
| **Access level** | `public`, `private`, atau `user` |
| **HTTP method** | `GET`, `POST`, `PUT`, `PATCH`, `DELETE` |
| **Skip activity log** | Tidak mencatat log endpoint ini ke Elasticsearch |

### Menulis query

Kolom query berisi SQL yang dijalankan.

```sql
SELECT * FROM table_name LIMIT 10
```

Untuk memakai **parameter**, tulis dengan format `{nama:tipe}`:

```sql
SELECT * FROM table_name WHERE user_id = {id:int}
```

### Advanced configuration

Endpoint database dapat memakai:

* request timeout
* rate limit
* request transformer
* log transformer
* request validation
* CORS

{% hint style="warning" %}
Endpoint database **tidak** memiliki response transformer.
{% endhint %}

## 3. Service database

| Field | Isi |
|---|---|
| **Path** | Bebas |
| **Connection type** | `SQL` |
| **Connection** | Connection yang sudah dibuat |
| **Endpoint** | Endpoint yang sudah dibuat |

**Contoh:** endpoint `/query` dapat disajikan sebagai service `/db/query`.
