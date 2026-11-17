# Konsep dasar

Untuk menyajikan sebuah layanan lewat AMP, Anda menyusun tiga hal secara berurutan.

```
 CONNECTION  →  ENDPOINT  →  SERVICE
 (ke mana)      (path asli)   (path yang dilihat konsumen)
```

| Konsep         | Arti                                                                | Contoh                                                               |
| -------------- | ------------------------------------------------------------------- | -------------------------------------------------------------------- |
| **Connection** | Alamat layanan tujuan (upstream)                                    | HTTPS: `https://httpbin.org`. DB: `your-db-ip:5432`                  |
| **Endpoint**   | Path **bawaan** upstream (atau path bebas untuk database dan skrip) | `/get`, `/post`, `/query`                                            |
| **Service**    | Path **hasil modifikasi AMP** yang dilihat konsumen                 | `/get` → `/http/get`, `/post` → `/http/post`, `/query` → `/db/query` |

## Jenis connection

| Jenis                                                                    | Fungsinya                                     | Halaman                                     |
| ------------------------------------------------------------------------ | --------------------------------------------- | ------------------------------------------- |
| HTTP                                                                     | Meneruskan request ke API lain                | [Koneksi HTTP](koneksi-http.md)             |
| Database (SQL)                                                           | Menjadikan query SQL sebagai API              | [Koneksi database](koneksi-database.md)     |
| JavaScript                                                               | Membuat endpoint dengan skrip, tanpa upstream | [Koneksi JavaScript](koneksi-javascript.md) |
| gRPC, GraphQL, Load Balancer, Proxy, Asynchronous, Event-driven, SAP RFC | Fitur baru AMP v3.0                           | [Koneksi baru](koneksi-baru-v3.md)          |

## Alur konsumen

Setelah service siap, atur siapa yang boleh memakainya lewat [Consumer](consumer.md):

```
 GROUP → CLIENT → CREDENTIAL → GRANT SERVICE
```
