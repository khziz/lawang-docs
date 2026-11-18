---
description: "Dokumentasi Lawang, terdiri dari AMP (API Gateway Management Platform) dan DSP (Data Synchronization Platform)."
---

# Lawang Documentation

**Lawang** terdiri dari dua platform yang saling melengkapi:

| Platform | Kepanjangan | Fungsi utama |
|---|---|---|
| **AMP** | API Gateway Management Platform | Mengelola API: menerima request, mengamankan, lalu meneruskannya ke layanan backend |
| **DSP** | Data Synchronization Platform | Menyinkronkan data antar database dan menjalankan job terjadwal |

Keduanya memakai **script JavaScript** yang sama. Di AMP, script ini adalah bagian dari **Connection JavaScript**, dengan kumpulan **method** `$GT` yang siap dipakai. Di DSP, script yang sama dipakai di dalam **job**.

{% hint style="info" %}
Semua nama (connection, job, topic, secret, URL, tabel) dalam contoh hanyalah **ilustrasi**. Sesuaikan dengan konfigurasi Anda.
{% endhint %}

## Struktur dokumentasi

```text
Lawang
├─ AMP
│   ├─ Overview & Architecture
│   ├─ Connection
│   │    ├─ HTTP
│   │    ├─ Database
│   │    ├─ JavaScript
│   │    │    └─ Method JavaScript ($GT)
│   │    └─ Fitur baru v3.0
│   ├─ Filter
│   ├─ Consumer
│   ├─ Settings
│   └─ Study Case AMP
└─ DSP
    ├─ Scheduler Job
    ├─ Sinkronisasi (Direct, Upsync, Downsync)
    ├─ Instalasi
    ├─ Membuat Job
    └─ Study Case DSP
```

## Pilih jalur Anda

| Anda ingin... | Mulai dari |
|---|---|
| Memahami gambaran besar | [Apa itu Lawang?](lawang/apa-itu-lawang.md) |
| Membuat API gateway | [AMP: Overview](amp/README.md) |
| Menghubungkan ke upstream atau database | [Connection](amp/connection/README.md) |
| Menulis script untuk endpoint | [Connection JavaScript](amp/connection/javascript/README.md) |
| Mencari satu method `$GT` | [Ringkasan semua method](amp/connection/javascript/methods/referensi/ringkasan-method.md) |
| Menyinkronkan data | [DSP: Overview](dsp/README.md) |
| Melihat contoh kasus nyata | [Study Case AMP](amp/study-case/README.md) · [Study Case DSP](dsp/study-case/README.md) |

## Info teknis method JavaScript

| | |
|---|---|
| Jumlah method `$GT` | 42 |
| Helper global | `atob`, `btoa`, `console.log` |
| Engine JavaScript | [Sobek](https://github.com/grafana/sobek) `v0.0.0-20261021132036-87a47258b7dc` |
| Snapshot sumber | `2ecee79c` |
