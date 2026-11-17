# Koneksi HTTP

Dipakai untuk meneruskan request ke **upstream HTTP/HTTPS**, misalnya API milik tim lain. Alurnya: buat **connection**, buat **endpoint**, lalu buat **service**.

## 1. Connection HTTP

| Field | Isi |
|---|---|
| **Name** | Bebas. Nama identitas upstream |
| **Base URL** | Domain atau IP upstream, misalnya `httpbin.org` |
| **Protocol** | `HTTP` atau `HTTPS`. Harus sama dengan protokol upstream |

### Authentication

Jika upstream butuh autentikasi, pilih salah satu **tipe**:

| Tipe | Field yang wajib diisi |
|---|---|
| **Basic** | `username`, `password` |
| **Key** | `key name`, `key value` |
| **Key & Token** | `key name`, `key value`, `token name`, `token value` |

**Authentication Location** punya dua opsi: **Header** atau **Queries**. Sesuaikan dengan kebutuhan upstream.

### Circuit breaker

Circuit breaker adalah **sakelar pengaman otomatis**. Ia menghentikan aliran request ke upstream yang sedang bermasalah, error, atau down.

Ada tiga fase:

| Fase | Arti |
|---|---|
| **Closed** | Sehat, request mengalir normal |
| **Open** | Tidak sehat, request dihentikan |
| **Half-Open** | Fase awal pulih setelah tidak sehat |

Pengaturannya:

| Setelan | Fungsi |
|---|---|
| **Max Concurrent Requests** | Jumlah maksimum request bersamaan ke upstream saat fase half-open |
| **Circuit Open Duration** | Waktu sampai Lawang kembali meneruskan request saat fase open. Jika upstream menjawab dengan benar, fase berubah menjadi half-open |
| **Monitoring Interval** | Waktu yang dibutuhkan untuk berpindah dari half-open ke closed |

## 2. Endpoint HTTP

| Field | Isi |
|---|---|
| **Connection type** | Pilih `HTTP` |
| **Connection** | Pilih connection yang sudah dibuat (otomatis difilter ke HTTP) |
| **API Path** | Path **asli** bawaan upstream |
| **Access level** | `public`, `private`, atau `user`. Pilih sesuai kebutuhan |
| **HTTP method** | Sesuai upstream: `GET`, `POST`, `PUT`, `PATCH`, `DELETE` |
| **Skip activity log** | Tidak mencatat log endpoint ini ke Elasticsearch |

### Konfigurasi lanjutan

| Setelan | Fungsi |
|---|---|
| **Forwarded request headers** | Header request yang diteruskan, misalnya `Authorization` |
| **Allow response header** | Mengizinkan header tertentu diteruskan ke response |
| **Request timeout** | Waktu maksimum dari memanggil service sampai menerima response |
| **Rate limit** | Jumlah total hit yang diizinkan |
| **Request transformer** | Mengubah request ([lihat Filter](filter.md)) |
| **Response transformer** | Mengubah response ([lihat Filter](filter.md)) |
| **Log transformer** | Mengubah isi log request atau response ([lihat Filter](filter.md)) |
| **Use CORS** | Mengatur CORS (lihat tabel berikut) |

### Pengaturan CORS

| Setelan | Fungsi |
|---|---|
| **Allowed Origins** | Domain yang boleh mengakses dari browser |
| **Max Age Duration** | Lama browser menyimpan hasil pengecekan CORS di cache |
| **Allowed Headers** | Header yang boleh dikirim |
| **Exposed Headers** | Header response yang boleh dibaca di browser |
| **Allow Credentials** | Mengizinkan pengiriman credential seperti session atau cookie |
| **Allow Private Network** | Mengizinkan request dari private network |

## 3. Service HTTP

| Field | Isi |
|---|---|
| **Path** | Bebas, ini path yang dilihat konsumen |
| **Connection type** | `HTTP` |
| **Connection** | Connection yang sudah dibuat |
| **Endpoint** | Endpoint yang sudah dibuat |

**Contoh:** endpoint `/get` pada upstream `https://httpbin.org` dapat disajikan sebagai service `/http/get`.
