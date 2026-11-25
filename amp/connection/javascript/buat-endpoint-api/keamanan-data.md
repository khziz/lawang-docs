# Mengamankan data

**Mode:** gateway dan scheduler. **Anda akan belajar:** meng-hash, mengenkripsi, menandatangani, dan membuat token.

## Langkah 1: Pilih alat yang tepat

| Kebutuhan                                  | Method                                        | Catatan                         |
| ------------------------------------------ | --------------------------------------------- | ------------------------------- |
| Sidik jari data (hash)                     | `$GT.sha256`, `$GT.md5`                       | Hasil hex huruf kecil           |
| Bukti pesan tidak berubah (kunci bersama)  | `$GT.hmac256`, `$GT.hmac512`                  | Dua pihak punya kunci yang sama |
| Menyembunyikan data (bisa dibuka lagi)     | `$GT.encrypt`, `$GT.decrypt`                  | Kunci dikelola di layanan pusat |
| Tanda tangan digital (kunci privat/publik) | `$GT.sha256_signing`, `$GT.sha256_verify`     | RSA                             |
| Token login/akses                          | `$GT.jwtrs256_signing`, `$GT.jwtrs256_verify` | JWT RS256                       |

## Hash

```js
$GT.sha256('Halo');
$GT.sha256(JSON.stringify({id: 7}));   // hash JSON: ubah jadi teks dulu
```

{% hint style="warning" %}
Object langsung (`$GT.sha256({id: 7})`) menjadi teks `"[object Object]"`. Selalu `JSON.stringify` dulu.
{% endhint %}

## HMAC

```js
$GT.hmac256('pesan', 'kunci-contoh');          // hex
$GT.hmac256('pesan', 'kunci-contoh', true);    // Base64 (tepat 3 argumen)
$GT.hmac512('pesan', 'kunci-contoh');          // selalu Base64
```

## Enkripsi dan dekripsi

Argumen pertama adalah **nama secret** di layanan pusat, bukan kuncinya.

```js
const cipher = $GT.encrypt('app-secret', JSON.stringify({id: 7}));
const data = JSON.parse($GT.decrypt('app-secret', cipher));
```

* Pakai secret yang **sama** untuk `encrypt` dan `decrypt`.
* Ciphertext berupa Base64 **tanpa padding**.
* Secret yang tidak ada atau Base64 yang rusak melempar error.

## Tanda tangan digital (RSA)

```js
// Menandatangani (kunci privat PKCS#8, "BEGIN PRIVATE KEY")
const signature = $GT.sha256_signing('pesan', privateKeyPem);

// Memverifikasi (kunci publik PKIX, "BEGIN PUBLIC KEY")
const valid = $GT.sha256_verify(signature, 'pesan', publicKeyPem);  // true / false
```

* Urutan verifikasi: **signature, pesan, kunci**.
* Pesan harus **identik byte demi byte**.
* Kunci atau Base64 yang tidak valid melempar error (bukan `false`).

## JWT

### Membuat token

```js
const now = Math.floor(Date.now() / 1000);   // detik
const token = $GT.jwtrs256_signing({
  sub: 'user-7', iat: now, exp: now + 300,
  iss: 'example-app', aud: 'example-api'
}, privateKeyPem);
```

Method ini **tidak** menambahkan `exp`, `iss`, atau `aud` otomatis. Tulis sendiri.

### Memeriksa token

```js
const claims = JSON.parse($GT.jwtrs256_verify(token, publicKeyPem));
if (claims.iss !== 'example-app') { throw 'issuer tidak sesuai'; }
```

{% hint style="danger" %}
`jwtrs256_verify` **tidak** mengecek `iss` (penerbit) atau `aud` (tujuan). Selalu periksa sendiri setelah `JSON.parse`.
{% endhint %}

Hasilnya adalah **teks JSON**, jadi pakai `JSON.parse`.

**Referensi:** [Hash, enkripsi, & tanda tangan](../ringkasan-method/keamanan.md)
