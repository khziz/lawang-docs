---
description: Overview AMP (API Gateway Management Platform) v3.0 dan peta isi bagian AMP.
---

# AMP: API Gateway Management Platform

AMP adalah platform untuk mengelola API gateway Lawang. Versi yang dibahas di sini adalah **AMP v3.0**.

## Peta isi

| Bagian | Isinya |
|---|---|
| [Apa itu API gateway?](api-gateway.md) | Pengertian dan fungsi gateway |
| [Architecture & system requirements](arsitektur.md) | Komponen AMP, spesifikasi, software |
| [Konsep dasar](konsep-dasar.md) | Alur Connection → Endpoint → Service |
| **Connection** | |
| ↳ [Overview Connection](connection/README.md) | Jenis-jenis connection |
| ↳ [Connection HTTP](connection/http.md) | Meneruskan request ke upstream HTTP |
| ↳ [Connection Database](connection/database.md) | Mengubah query SQL menjadi API |
| ↳ [Connection JavaScript](connection/javascript/README.md) | Membuat endpoint dengan script |
| ↳ [Method JavaScript](connection/javascript/methods/README.md) | Kumpulan method `$GT` untuk script |
| ↳ [Fitur baru v3.0](connection/fitur-baru-v3.md) | gRPC, GraphQL, Load Balancer, Proxy, Asynchronous, Event-driven, SAP RFC |
| [Filter](filter.md) | Validation dan transformer untuk request, response, log |
| [Consumer](consumer.md) | Group, client, credential, grant service |
| [Settings](settings.md) | WAF, statistics server, general configs |
| [Study Case AMP](study-case/README.md) | Contoh kasus dari awal sampai akhir |

## Urutan kerja yang umum

1. Siapkan komponen AMP ([architecture](arsitektur.md)).
2. Buat **connection** ke layanan backend.
3. Buat **endpoint** (path asli atau script).
4. Buat **service** (path yang dilihat konsumen).
5. Pasang **filter** bila perlu.
6. Buat **consumer** dan beri akses.
7. Atur **settings** keamanan dan log.
