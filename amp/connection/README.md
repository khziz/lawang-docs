---
description: "Semua jenis connection di AMP dan cara memilihnya."
---

# Connection

**Connection** menyimpan informasi tujuan layanan: alamat, protokol, dan autentikasi. Setiap connection kemudian dipakai oleh **endpoint**.

## Semua jenis connection

| Connection | Fungsi | Halaman |
|---|---|---|
| **HTTP** | Meneruskan request ke API HTTP/HTTPS | [Connection HTTP](http.md) |
| **Database** | Menjadikan query SQL sebagai API (PostgreSQL, Oracle, MySQL) | [Connection Database](database.md) |
| **JavaScript** | Menulis logika sendiri dengan script, tanpa upstream | [Connection JavaScript](javascript/README.md) |
| **gRPC** | Komunikasi antar-service lewat remote procedure call | [Connection gRPC](grpc.md) |
| **GraphQL** | API berbasis query language | [Connection GraphQL](graphql.md) |
| **Load Balancer** | Membagi traffic ke beberapa server | [Connection Load Balancer](load-balancer.md) |
| **Proxy** | Perantara antara client dan server | [Connection Proxy](proxy.md) |
| **Asynchronous** | Mengirim HTTP request otomatis saat event terjadi | [Connection Asynchronous](asynchronous.md) |
| **Event-driven** | Proses yang dipicu event | [Connection Event-driven](event-driven.md) |
| **SAP RFC** | Komunikasi dengan SAP lewat SAP RFC Adapter | [Connection SAP RFC](sap-rfc.md) |

{% hint style="info" %}
Connection HTTP, Database, dan JavaScript sudah dilengkapi langkah konfigurasinya. Untuk connection lain, materi sumber saat ini baru memuat konsepnya.
{% endhint %}

## Perbandingan HTTP, Database, dan JavaScript

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
