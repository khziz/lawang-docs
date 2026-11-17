---
description: "Dokumentasi Lawang, terdiri dari AMP (API Gateway Management Platform) dan DSP (Data Synchronization Platform)."
---

# Lawang Documentation

**Lawang** terdiri dari dua platform yang saling melengkapi:

| Platform | Kepanjangan                   | Fungsi utama                                                                       |
| -------- | ----------------------------- | ---------------------------------------------------------------------------------- |
| **AMP**  | API Gateway Management System | Mengelola API: menerima request, mengamankan, dan meneruskannya ke layanan backend |
| **DSP**  | Data Synchronization Platform | Menyinkronkan data antar database dan menjalankan job terjadwal                    |

Keduanya memakai **skrip JavaScript** yang sama (kotak peralatan `$GT`). Skrip ini adalah salah satu fitur di **koneksi JavaScript** pada AMP, dan dipakai juga pada job di DSP.

{% hint style="info" %}
Semua nama (koneksi, job, topic, secret, URL, tabel) dalam contoh hanyalah **ilustrasi**. Sesuaikan dengan konfigurasi Anda.
{% endhint %}

## Pilih jalur Anda

| Anda ingin...                                       | Mulai dari                                                                 |
| --------------------------------------------------- | -------------------------------------------------------------------------- |
| Memahami gambaran besar Lawang                      | [Apa itu Lawang?](apa-itu-lawang.md)                                       |
| Menyiapkan API gateway (koneksi, endpoint, service) | [AMP: Pengantar](amp-api-gateway/amp.md)                                   |
| Mengatur siapa yang boleh memanggil API             | [Consumer](amp-api-gateway/consumer.md)                                    |
| Menulis skrip JavaScript untuk endpoint             | [Koneksi JavaScript](amp-api-gateway/koneksi-javascript.md)                |
| Menyinkronkan data antar database                   | [DSP: Pengantar](dsp-data-synchronization/dsp.md)                          |
| Membuat job terjadwal                               | [Membuat server, jadwal, dan job](dsp-data-synchronization/membuat-job.md) |
| Mencari detail satu fungsi `$GT`                    | [Ringkasan semua fungsi](referensi-fungsi/ringkasan-fungsi.md)             |

## Urutan belajar yang disarankan

1. [**Apa itu Lawang?**](apa-itu-lawang.md): pahami peran AMP dan DSP.
2. **AMP**: arsitektur, konsep koneksi, endpoint, service, lalu consumer.
3. **Koneksi JavaScript**: tulis skrip sendiri dengan panduan `$GT`.
4. **DSP**: instalasi, sinkronisasi data, dan job terjadwal.

## Info teknis fungsi JavaScript

|                     |                                                                                |
| ------------------- | ------------------------------------------------------------------------------ |
| Jumlah fungsi `$GT` | 42                                                                             |
| Helper global       | `atob`, `btoa`, `console.log`                                                  |
| Engine JavaScript   | [Sobek](https://github.com/grafana/sobek) `v0.0.0-20261021132036-87a47258b7dc` |
| Snapshot sumber     | `2ecee79c`                                                                     |
