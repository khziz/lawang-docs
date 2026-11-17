---
description: "Overview jenis-jenis connection di AMP dan cara memilihnya."
---

# Connection

**Connection** menyimpan informasi tujuan layanan: alamat, protokol, dan autentikasi. Setiap connection kemudian dipakai oleh **endpoint**.

## Pilih jenis connection

| Kebutuhan | Jenis | Halaman |
|---|---|---|
| Meneruskan request ke API HTTP/HTTPS | **HTTP** | [Connection HTTP](http.md) |
| Menjadikan query SQL sebagai API | **Database** (PostgreSQL, Oracle, MySQL) | [Connection Database](database.md) |
| Menulis logika sendiri dengan script | **JavaScript** | [Connection JavaScript](javascript/README.md) |
| gRPC, GraphQL, Load Balancer, Proxy, Asynchronous, Event-driven, SAP RFC | **Fitur baru v3.0** | [Fitur baru v3.0](fitur-baru-v3.md) |

## Perbandingan

| | HTTP | Database | JavaScript |
|---|---|---|---|
| Butuh upstream? | Ya (base URL) | Ya (host database) | **Tidak** |
| Isi endpoint | API path asli upstream | Query SQL | Script |
| Request validation | Ya | Ya | Ya |
| Request transformer | Ya | Ya | Ya |
| Response transformer | Ya | **Tidak** | **Tidak** |

## Urutan membuat

1. Buat **connection** (tujuan).
2. Buat **endpoint** (path atau script, access level, HTTP method).
3. Buat **service** (path yang dilihat consumer).

Konsepnya ada di [Konsep dasar](../konsep-dasar.md).
