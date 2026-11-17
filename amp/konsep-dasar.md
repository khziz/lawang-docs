---
description: "Tiga konsep inti AMP, yaitu Connection, Endpoint, dan Service."
---

# Konsep dasar: Connection, Endpoint, Service

Untuk menyajikan sebuah layanan lewat AMP, Anda menyusun tiga hal secara berurutan.

```text
 CONNECTION  →  ENDPOINT   →  SERVICE
 (ke mana)      (path asli)    (path yang dilihat consumer)
```

| Konsep | Arti | Contoh |
|---|---|---|
| **Connection** | Alamat layanan tujuan (upstream) | HTTPS: `https://httpbin.org`. Database: `your-db-ip:5432` |
| **Endpoint** | Path **bawaan** upstream (atau path bebas untuk database dan JavaScript) | `/get`, `/post`, `/query` |
| **Service** | Path **hasil modifikasi AMP** yang dilihat consumer | `/get` → `/http/get`, `/post` → `/http/post`, `/query` → `/db/query` |

## Jenis connection

| Jenis | Fungsinya | Halaman |
|---|---|---|
| HTTP | Meneruskan request ke API lain | [Connection HTTP](connection/http.md) |
| Database (SQL) | Menjadikan query SQL sebagai API | [Connection Database](connection/database.md) |
| JavaScript | Membuat endpoint dengan script, tanpa upstream | [Connection JavaScript](connection/javascript/README.md) |
| gRPC, GraphQL, Load Balancer, Proxy, Asynchronous, Event-driven, SAP RFC | Fitur baru AMP v3.0 | [Fitur baru v3.0](connection/fitur-baru-v3.md) |

## Alur consumer

Setelah service siap, atur siapa yang boleh memakainya lewat [Consumer](consumer.md):

```text
 GROUP → CLIENT → CREDENTIAL → GRANT SERVICE
```
