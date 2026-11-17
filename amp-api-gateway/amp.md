# AMP: Pengantar

AMP adalah platform untuk mengelola API gateway Lawang. Versi yang dibahas di sini adalah **AMP v3.0**.

## Isi bagian ini

| Halaman                                          | Isinya                                                            |
| ------------------------------------------------ | ----------------------------------------------------------------- |
| [Apa itu API gateway?](api-gateway.md)           | Pengertian dan fungsi gateway                                     |
| [Arsitektur dan kebutuhan sistem](arsitektur.md) | Komponen AMP, spesifikasi, software                               |
| [Konsep dasar](konsep-dasar.md)                  | Alur Connection → Endpoint → Service                              |
| [Koneksi HTTP](koneksi-http.md)                  | Meneruskan request ke upstream HTTP                               |
| [Koneksi database](koneksi-database.md)          | Mengubah query SQL menjadi API                                    |
| [Koneksi JavaScript](koneksi-javascript.md)      | Membuat endpoint dengan skrip                                     |
| [Koneksi baru di AMP v3.0](koneksi-baru-v3.md)   | gRPC, GraphQL, Load Balancer, Proxy, Async, Event-driven, SAP RFC |
| [Filter](filter.md)                              | Validasi dan transformasi request, response, log                  |
| [Consumer](consumer.md)                          | Group, client, credential, grant service                          |
| [Pengaturan](pengaturan.md)                      | WAF, statistik server, konfigurasi umum                           |

## Urutan kerja yang umum

1. Siapkan komponen AMP ([arsitektur](arsitektur.md)).
2. Buat **connection** ke layanan backend.
3. Buat **endpoint** (path asli atau skrip).
4. Buat **service** (path yang dilihat konsumen).
5. Pasang **filter** bila perlu.
6. Buat **consumer** dan beri akses.
7. Atur **pengaturan** keamanan dan log.
