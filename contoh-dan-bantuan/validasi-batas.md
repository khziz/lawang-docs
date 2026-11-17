# Validasi & batas dokumentasi

## Cara dokumen ini divalidasi

* Inventaris dibandingkan dengan `configs.FunctionsMap`, loader, serta registrasi global dan scheduler.
* Tipe parameter dan hasil ditelusuri ke `javascripts/functions`, `files`, `jsons`, dan `utils` bila dibutuhkan.
* Contoh memakai data ilustratif. Integrasi database, HTTP, SAP, HSM, agent, dan konfigurasi pusat **tidak dijalankan**.

## Kejanggalan yang tercatat

Dokumen ini merekam perilaku **source snapshot**, termasuk batas dan kejanggalan yang ditemukan:

| Area                  | Ringkasan                                                |
| --------------------- | -------------------------------------------------------- |
| `$GT.mask`            | `end` >= panjang byte dapat memicu exception indeks      |
| `$GT.jwtrs256_verify` | Hasil berupa string JSON claims, bukan object            |
| `$GT.rfc`             | Parser `output` tidak aman bila diisi integer            |
| `$GT.rfc_sync`        | `parameters` wajib ada, dan `target.agent` tidak dipakai |

Tidak ada perubahan pada implementasi Go. Pengujian runtime, `betteralign`, dan benchmark tidak berlaku untuk pembuatan dokumentasi ini.

## Sumber

Snapshot `2ecee79c`, engine `github.com/grafana/sobek v0.0.0-20261021132036-87a47258b7dc`.
