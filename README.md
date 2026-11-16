# Lawang Core: Referensi JavaScript

Panduan fungsi JavaScript untuk **API gateway** dan **job scheduler** Lawang Core. Di sini Anda menemukan sintaks, parameter, nilai kembalian, dan contoh pemakaian setiap fungsi.

| | |
|---|---|
| Fungsi `$GT` | 42 |
| Helper global | 3 (`atob`, `btoa`, `console.log`) |
| Engine | [Sobek](https://github.com/grafana/sobek) `v0.0.0-20261021132036-87a47258b7dc` |
| Snapshot sumber | `2ecee79c` |
| Tanggal dokumen | 23 Oktober 2026 |

{% hint style="info" %}
Semua nama backend, job, topic, secret, URL, tabel, dan kunci dalam contoh hanyalah **ilustrasi**. Sesuaikan dengan konfigurasi Anda. Contoh yang mengubah sistem luar (database, file, publish, sync) tidak dieksekusi saat dokumen ini disusun.
{% endhint %}

## Mulai dari mana?

- **Baru pertama kali?** Baca [Cara membaca dokumen & aturan runtime](panduan/cara-membaca.md).
- **Ingin tahu data request apa yang tersedia?** Lihat [Properti konteks `$GT`](panduan/properti-konteks.md).
- **Mencari fungsi tertentu?** Buka [Ringkasan semua fungsi](ringkasan-fungsi.md).
- **Ingin contoh siap pakai?** Lihat [Contoh alur lengkap](contoh/alur-lengkap.md).

## Gateway vs Scheduler

Lawang menjalankan skrip di dua mode yang terpisah:

| Mode | Fungsinya | Fungsi khusus mode ini |
|---|---|---|
| **Gateway** | Menangani request HTTP masuk | `$GT.response` |
| **Scheduler** | Menjalankan job terjadwal | `$GT.run`, `$GT.parallel`, `$GT.sync`, `$GT.rfc_sync` |

Fungsi lainnya tersedia di kedua mode.
