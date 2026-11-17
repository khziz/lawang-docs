# Kasus 2: Query database sebagai API

## Skenario

Tim lain butuh data user dari database PostgreSQL Anda, tapi Anda tidak ingin membuka akses database langsung. Anda membuat API read-only yang menjalankan satu query.

## Komponen

| Komponen | Nilai contoh |
|---|---|
| Connection Database | `db-main` (PostgreSQL) |
| Endpoint | `/query` (GET) |
| Service | `/db/query` |
| Filter | Request validation |

## Langkah

### 1. Buat Connection Database

| Field | Isi |
|---|---|
| Name | `db-main` |
| Host | `db-host:5432?sslmode=disable` |
| Driver | `PostgreSQL` |
| Database, username, password | Sesuai credential database |

### 2. Buat Endpoint

| Field | Isi |
|---|---|
| Connection type | `SQL` |
| Connection | `db-main` |
| API Path | `/query` |
| Access level | `user` |
| HTTP method | `GET` |

Isi kolom query dengan parameter berformat `{nama:tipe}`:

```sql
SELECT * FROM table_name WHERE user_id = {id:int}
```

### 3. Pasang request validation

Pastikan consumer selalu mengirim `id` berupa angka, satu nilai saja.

```json
{
  "queries": {
    "schema": {
      "type": "object",
      "required": ["id"],
      "properties": {
        "id": {
          "type": "array",
          "minItems": 1,
          "maxItems": 1,
          "items": {
            "type": "string",
            "pattern": "^\\d+$"
          }
        }
      }
    },
    "message": "Invalid Parameters",
    "code": 400
  }
}
```

Request tanpa `id`, atau dengan `id` yang bukan angka, akan ditolak dengan `400 Invalid Parameters`.

### 4. Buat Service

| Field | Isi |
|---|---|
| Path | `/db/query` |
| Connection type | `SQL` |
| Connection | `db-main` |
| Endpoint | Endpoint `/query` tadi |

## Hasil

Consumer memanggil `/db/query?id=7` dan menerima hasil query. Query dijalankan oleh AMP, sehingga consumer tidak pernah memegang credential database.

## Catatan

* Endpoint database **tidak** punya response transformer.
* Untuk logika yang lebih kompleks (beberapa query, validasi bertahap), pakai [Connection JavaScript](03-crud-items-javascript.md).
