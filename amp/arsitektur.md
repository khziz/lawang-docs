---
description: "Komponen AMP, kebutuhan hardware, dan software yang dibutuhkan."
---

# Arsitektur dan kebutuhan sistem

## Komponen AMP

| Komponen | Fungsi |
|---|---|
| **Core Engine Cluster** | Memproses request, memvalidasi, meneruskan ke handler yang sesuai, dan mengembalikan response ke consumer |
| **Load Balancer** | Mengatur pembagian traffic ke Core sesuai algoritma yang ditetapkan |
| **Console** | Konfigurasi: membuat connection, endpoint, service, dan mengatur consumer |
| **Dashboard Monitoring** | Menelusuri (tracing) request, memeriksa error, dan memvisualisasikan traffic |

## Kebutuhan sistem

Tiga paket spesifikasi tersedia:

| Komponen | Minimum | Medium | Best |
|---|---|---|---|
| **Console + Dashboard** (digabung) | 2 core CPU, 4 GB RAM, 180 GB storage | n/a | n/a |
| **Console** (terpisah) | n/a | 1 core CPU, 1 GB RAM, 180 GB storage | 2 core CPU, 4 GB RAM, 180 GB storage |
| **Dashboard Monitoring** (terpisah) | n/a | 2 core CPU, 4 GB RAM, 180 GB storage | 4 core CPU, 8 GB RAM, 180 GB storage |
| **Core** | 1 core CPU, 1 GB RAM, 180 GB storage | 1 core CPU, 1 GB RAM, 180 GB storage | 2 core CPU, 4 GB RAM, 180 GB storage |

Pada paket **Minimum**, Console dan Dashboard dijalankan di satu mesin. Pada **Medium** dan **Best**, keduanya dipisah.

## Sistem operasi

Linux, Windows, macOS, dan sebagainya.

## Software yang dibutuhkan

| Komponen | Software |
|---|---|
| Console | PostgreSQL (terbaru), Docker Compose (terbaru) |
| Dashboard Monitoring | Elasticsearch (terbaru), Kibana (terbaru) |
| Core | Docker Compose (terbaru) |
