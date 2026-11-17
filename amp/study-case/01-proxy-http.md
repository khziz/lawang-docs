# Kasus 1: Meneruskan request ke API partner

## Skenario

Aplikasi internal perlu memanggil API milik partner (di contoh ini `https://httpbin.org`). Anda ingin semua panggilan lewat AMP supaya ada **rate limit**, **timeout**, **CORS**, dan pencatatan log di satu tempat.

## Komponen

| Komponen | Nilai contoh |
|---|---|
| Connection HTTP | `partner-api` |
| Endpoint | `/get` (GET) |
| Service | `/http/get` |

## Langkah

### 1. Buat Connection HTTP

| Field | Isi |
|---|---|
| Name | `partner-api` |
| Base URL | `httpbin.org` |
| Protocol | `HTTPS` |
| Authentication | Sesuai kebutuhan upstream (Basic, Key, atau Key & Token), location Header atau Queries |

Aktifkan **circuit breaker** agar request dihentikan otomatis saat upstream bermasalah. Lihat [Connection HTTP](../connection/http.md#circuit-breaker).

### 2. Buat Endpoint

| Field | Isi |
|---|---|
| Connection type | `HTTP` |
| Connection | `partner-api` |
| API Path | `/get` (path asli upstream) |
| Access level | `user` (supaya bisa di-grant per client) |
| HTTP method | `GET` |

Advanced configuration:

| Setelan | Contoh |
|---|---|
| Forwarded request headers | `Authorization` |
| Request timeout | Batas waktu sampai response diterima |
| Rate limit | Jumlah hit yang diizinkan |
| Use CORS | Isi Allowed Origins sesuai domain aplikasi web Anda |

### 3. Buat Service

| Field | Isi |
|---|---|
| Path | `/http/get` |
| Connection type | `HTTP` |
| Connection | `partner-api` |
| Endpoint | Endpoint `/get` tadi |

### 4. Beri akses ke consumer

Buat group, client, dan credential, lalu daftarkan endpoint di **grant service**. Langkahnya ada di [Kasus 5](05-akses-per-departemen.md).

## Hasil

Consumer memanggil `/http/get` di AMP. AMP meneruskan ke `https://httpbin.org/get` dengan aturan timeout, rate limit, dan CORS yang Anda set.

## Catatan

* Hanya endpoint dengan access level `user` yang bisa dimasukkan ke grant service.
* Gunakan **Skip activity log** hanya bila endpoint tidak perlu dicatat ke Elasticsearch.
