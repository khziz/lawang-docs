---
description: Kumpulan method $GT untuk script JavaScript di AMP dan DSP.
---

# Method JavaScript ($GT)

Bagian ini membahas kumpulan **method** `$GT` yang siap dipakai di script JavaScript. Method dipakai pada **[Connection JavaScript](../README.md) di AMP** (membuat endpoint) dan pada **job di [DSP](../../../../dsp/README.md)** (tugas terjadwal dan sinkronisasi).

{% hint style="info" %}
**Method** di sini berarti method `$GT.nama(...)`, misalnya `$GT.query`. Jangan tertukar dengan **HTTP method** (GET, POST, dan seterusnya).
{% endhint %}

{% hint style="info" %}
Semua nama (connection database, job, topic, secret, URL, tabel) dalam contoh hanya **ilustrasi**. Ganti dengan nama yang ada di konfigurasi Anda.
{% endhint %}

## Pilih jalur Anda

| Anda ingin... | Mulai dari |
|---|---|
| Tahu Lawang itu apa | [Mengenal script JavaScript](mulai/mengenal-script.md) |
| Langsung mencoba | [Script pertama Anda](mulai/script-pertama.md) |
| Mengerjakan tugas tertentu (database, API, file, dll.) | Bagian **Panduan per tugas** di sidebar |
| Mencari detail satu method | [Ringkasan semua method](referensi/ringkasan-method.md) |
| Mencari tahu kenapa script error | [Troubleshooting](bantuan/pemecahan-masalah.md) |

## Urutan belajar yang disarankan

1. **Mulai di sini:** kenali konsep dan jalankan script pertama.
2. **Konsep dasar:** pahami aturan yang berlaku untuk semua script.
3. **Panduan per tugas:** pilih tugas yang Anda butuhkan, ikuti langkahnya.
4. **Referensi:** buka saat butuh detail lengkap sebuah method.
5. **Bantuan:** daftar jebakan umum dan cara mengatasi error.

## Info teknis

| | |
|---|---|
| Jumlah method `$GT` | 42 |
| Helper global | `atob`, `btoa`, `console.log` |
| Engine JavaScript | [Sobek](https://github.com/grafana/sobek) `v0.0.0-20261021132036-87a47258b7dc` |
| Snapshot sumber | `2ecee79c` |
| Tanggal dokumen sumber | 23 Oktober 2026 |
