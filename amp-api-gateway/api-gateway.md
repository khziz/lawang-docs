# Apa itu API gateway?

**API gateway** adalah lapisan perangkat lunak yang menyediakan **titik masuk tunggal** bagi klien (seperti aplikasi web atau mobile) untuk mengakses beberapa layanan backend, sekaligus mengelola interaksi antara klien dan server. _(Sumber definisi: IBM)_

## Analogi singkat

Bayangkan resepsionis gedung. Tamu tidak langsung masuk ke tiap kantor. Mereka melapor ke resepsionis, yang memeriksa identitas, lalu mengarahkan ke ruangan yang benar. API gateway berperan sebagai resepsionis itu.

## Fungsi gateway

| Fungsi                         | Arti                                                   |
| ------------------------------ | ------------------------------------------------------ |
| Request routing                | Mengarahkan request ke layanan yang tepat              |
| Authentication & authorization | Memastikan siapa pemanggil dan apa haknya              |
| Rate limiting                  | Membatasi jumlah request                               |
| Security                       | Melindungi layanan dari serangan                       |
| Logging                        | Mencatat aktivitas request                             |
| Monitoring                     | Memantau kondisi dan lalu lintas                       |
| Caching                        | Menyimpan respons agar request berulang lebih cepat    |
| Circuit breaker                | Menghentikan request ke layanan yang sedang bermasalah |
| Protocol translation           | Menerjemahkan antar protokol                           |

Dan fungsi lainnya.

Di AMP, setiap fungsi di atas diatur lewat konfigurasi connection, endpoint, filter, consumer, dan pengaturan sistem.
