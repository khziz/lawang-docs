# Jebakan umum

Kumpulan kesalahan yang paling sering terjadi, dan cara yang benar.

## Pemanggilan method

| Salah | Yang benar | Penjelasan |
|---|---|---|
| `await $GT.query(...)` | `$GT.query(...)` | Semua method sinkron |
| `$GT[nama](...)` | `$GT.query(...)` | Method bisa tidak dimuat |
| `fetch(url)` | `$GT.get(url)` | `fetch` tidak ada |
| `$GT.log('a', 'b')` | `$GT.log({a: 'a', b: 'b'})` | `log` hanya menerima **satu** nilai, jika tidak hasilnya `false` |

## HTTP

| Salah | Yang benar | Penjelasan |
|---|---|---|
| Mengira error 404 melempar exception | Cek `res.code` | Gagal HTTP tidak melempar |
| `$GT.post(url, 5)` ingin body angka | Beri body sebagai object/string | Integer di slot body dianggap **timeout** |
| Timeout di tengah argumen | Taruh di akhir | Slot berikutnya bisa menimpanya |
| `$GT.response` di scheduler | Hanya di gateway | |

## Database

| Salah | Yang benar | Penjelasan |
|---|---|---|
| `$GT.save(src, tbl, {id: 1})` | `$GT.save(src, tbl, [{id: 1}])` | `save` butuh **array** |
| `$GT.update(src, tbl, [7], ...)` | `$GT.update(src, tbl, ['id'], ...)` | `identities` adalah **nama kolom** |
| `$GT.exists(src, tbl, '{"id":7}')` | `$GT.exists(src, tbl, {id: 7})` | Payload harus object |
| Menyambung SQL dengan string | Pakai `?` dan array parameter | Hindari penyusupan SQL |
| Berharap `exec` memberi jumlah baris | Pakai `query` untuk menghitung | `exec` hanya mengembalikan `null` |
| Mengandalkan `exists` = `false` berarti "tidak ada" | Ingat bisa juga pemeriksaan gagal | |
| Membaca lebih dari 1.000 baris | Batasi/halaman query | `query` maksimum 1.000 baris |
| ID `int64` besar | Kirim sebagai teks bila perlu | Bisa kehilangan presisi di JS |

## File

| Salah | Yang benar | Penjelasan |
|---|---|---|
| `store_file(src, path, base64)` | `store_file(src, base64, path)` | **Isi dulu, path kemudian** |
| `if ($GT.check_file(...))` untuk cek ada | `try/catch` | Hasilnya **ukuran** (angka), dan file hilang melempar error |
| Menganggap `read_csv` mengubah angka | Ubah dengan `Number(...)` | Semua nilai tetap teks |
| `read_csv` memotong header otomatis | `.slice(1)` | Header tetap baris pertama |
| `read_excel(src, path, 'Sheet2')` | `read_excel(src, path, 1)` | Argumen ketiga adalah **indeks** (mulai 0) |

## Keamanan

| Salah | Yang benar | Penjelasan |
|---|---|---|
| `$GT.sha256({id: 7})` | `$GT.sha256(JSON.stringify({id: 7}))` | Object menjadi `"[object Object]"` |
| `$GT.encrypt('kunci-mentah', ...)` | `$GT.encrypt('nama-secret', ...)` | Parameter pertama = **nama** secret |
| `sha256_verify(pesan, signature, key)` | `sha256_verify(signature, pesan, key)` | Signature dulu |
| Kunci privat format PKCS#1 untuk `sha256_signing` | Format PKCS#8 | `BEGIN PRIVATE KEY` |
| Mengira `jwtrs256_verify` mengecek `iss`/`aud` | Cek sendiri setelah `JSON.parse` | Tidak ada pengecekan otomatis |
| `hmac256(..., true, 'x')` | Tepat 3 argumen untuk Base64 | Dengan 4+ argumen hasil kembali hex |

## Masking dan transformasi

| Salah | Yang benar | Penjelasan |
|---|---|---|
| `mask('123456', '*', 0, 6)` | `mask('123456', '*', 0, 5)` | `end` inklusif, maksimum panjang − 1 |
| Masking teks non-ASCII | Gunakan ASCII | Bekerja per **byte** |
| `transform({..}, spec)` | `transform(JSON.stringify({..}), spec)` | Argumen pertama harus **teks JSON** |

## Pesan, SAP, scheduler

| Salah | Yang benar | Penjelasan |
|---|---|---|
| `publish(src, topic, '{"id":7}')` | `publish(src, topic, {id: 7})` | String JSON ditolak |
| `rfc_sync` tanpa `parameters` | `parameters: {}` | Parser berhenti sebelum `output` |
| `rfc(..., {}, 10)` diganti `rfc(..., 10)` | Beri `{}` di slot output | Integer di slot output tidak aman |
| Agent di kedua sisi `sync` | Agent di **satu** sisi | Bukan kontrak relay |
| Mengira `parallel` mengembalikan hasil tiap job | Hanya `null` atau error | Bukan `Promise.all` |
| `$GT.run('x', false)` menyembunyikan semua error | Job tidak ditemukan tetap throw | `false` hanya untuk error eksekusi |
