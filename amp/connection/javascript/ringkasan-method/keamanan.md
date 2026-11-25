# Hash, enkripsi, & tanda tangan

{% hint style="success" %}
Baru belajar? Baca dulu [panduan langkah demi langkah](../buat-endpoint-api/keamanan-data.md). Halaman ini adalah **referensi teknis** untuk dicari ulang.
{% endhint %}

**Mode:** gateway dan scheduler, untuk semua method di halaman ini.

## Pilih method yang tepat

| Kebutuhan                                 | Method                                |
| ----------------------------------------- | ------------------------------------- |
| Hash biasa                                | `md5`, `sha256`                       |
| Autentikasi pesan dengan kunci bersama    | `hmac256`, `hmac512`                  |
| Enkripsi/dekripsi dengan secret terkelola | `encrypt`, `decrypt`                  |
| Tanda tangan RSA atas teks                | `sha256_signing`, `sha256_verify`     |
| Token JWT RS256                           | `jwtrs256_signing`, `jwtrs256_verify` |

## Hash

### `$GT.md5` dan `$GT.sha256`

```js
$GT.md5(value)
$GT.sha256(value)
```

| Parameter | Keterangan                                 |
| --------- | ------------------------------------------ |
| `value`   | Dikonversi menjadi string, lalu byte UTF-8 |

**Hasil:** string digest heksadesimal huruf kecil. Tanpa parameter akan menghasilkan exception.

{% hint style="warning" %}
Object biasa menjadi `"[object Object]"`. Gunakan `JSON.stringify` untuk meng-hash JSON.
{% endhint %}

```js
$GT.md5('Halo');
$GT.md5('');
$GT.md5(123);                       // sama dengan string '123'
$GT.md5(JSON.stringify({id: 7}));

$GT.sha256('Halo');
$GT.sha256(JSON.stringify({id: 7}));
```

## HMAC

### `$GT.hmac256`

```js
$GT.hmac256(message, key, base64?)
```

| Parameter        | Keterangan                |
| ---------------- | ------------------------- |
| `message`, `key` | Dikonversi menjadi string |
| `base64`         | boolean, default `false`  |

**Hasil:** HMAC-SHA256, default **hex huruf kecil**. Base64 standar jika argumen ketiga truthy.

{% hint style="warning" %}
Mode Base64 hanya dibaca ketika memakai **tepat tiga argumen**. Jika ada empat atau lebih, hasil kembali hex. Jangan menambah argumen yang tidak didokumentasikan.
{% endhint %}

```js
$GT.hmac256('pesan', 'kunci-contoh');
$GT.hmac256('pesan', 'kunci-contoh', false); // hex
$GT.hmac256('pesan', 'kunci-contoh', true);  // Base64
$GT.hmac256('', 'kunci-contoh', true);
```

### `$GT.hmac512`

```js
$GT.hmac512(message, key)
```

**Hasil:** HMAC-SHA512 dalam **Base64 standar**. Tidak ada opsi hex, dan argumen tambahan tidak mengubah format.

```js
$GT.hmac512('pesan', 'kunci-contoh');
$GT.hmac512(JSON.stringify({id: 7}), 'kunci-contoh');
$GT.hmac512('', 'kunci-contoh');
```

## Enkripsi dengan secret

### `$GT.encrypt`

```js
$GT.encrypt(secretName, plaintext)
```

| Parameter    | Keterangan                                                |
| ------------ | --------------------------------------------------------- |
| `secretName` | **Nama** secret di konfigurasi pusat (bukan kunci mentah) |
| `plaintext`  | Dikonversi menjadi string                                 |

**Hasil:** ciphertext Base64 standar **tanpa padding** (RawStdEncoding).

Secret dapat memakai encryptor software atau HSM sesuai konfigurasi. Secret atau encryptor yang tidak tersedia menghasilkan exception.

```js
const encrypted = $GT.encrypt('app-secret', 'Halo');
const encryptedJson = $GT.encrypt('app-secret',
  JSON.stringify({id: 7}));
$GT.encrypt('app-secret', '');
```

### `$GT.decrypt`

```js
$GT.decrypt(secretName, ciphertext)
```

| Parameter    | Keterangan                                 |
| ------------ | ------------------------------------------ |
| `secretName` | Secret yang **sama** dengan saat `encrypt` |
| `ciphertext` | Base64 tanpa padding hasil `encrypt`       |

**Hasil:** string plaintext. Base64 salah atau kegagalan HSM menghasilkan exception.

{% hint style="warning" %}
Perilaku ciphertext tidak valid pada encryptor software mengikuti encryptor itu sendiri. Ini **bukan** jaminan validasi autentikasi.
{% endhint %}

```js
const cipher = $GT.encrypt('app-secret', 'Halo');
$GT.decrypt('app-secret', cipher); // "Halo"

const cipherJson = $GT.encrypt('app-secret', '{"id":7}');
const data = JSON.parse($GT.decrypt('app-secret', cipherJson));

try { $GT.decrypt('app-secret', '%%%'); }
catch (err) { $GT.log(String(err)); }
```

## Tanda tangan RSA

### `$GT.sha256_signing`

```js
$GT.sha256_signing(message, privateKeyPem)
```

| Parameter       | Keterangan                                                   |
| --------------- | ------------------------------------------------------------ |
| `message`       | String                                                       |
| `privateKeyPem` | PEM kunci privat RSA format **PKCS#8** (`BEGIN PRIVATE KEY`) |

**Hasil:** signature Base64 standar.

Menandatangani `SHA-256(message)` dengan **RSA PKCS#1 v1.5** (bukan RSA-PSS). Kunci PKCS#1 langsung **tidak diterima**. Kunci tidak valid melempar exception.

```js
// privateKeyPem disediakan oleh script/konfigurasi Anda.
const signature = $GT.sha256_signing('pesan', privateKeyPem);
const body = JSON.stringify({id: 7});
const jsonSignature = $GT.sha256_signing(body, privateKeyPem);
```

### `$GT.sha256_verify`

```js
$GT.sha256_verify(signature, message, publicKeyPem)
```

| Parameter      | Keterangan                                              |
| -------------- | ------------------------------------------------------- |
| `signature`    | Base64 standar                                          |
| `message`      | Pesan asli                                              |
| `publicKeyPem` | PEM RSA public key format **PKIX** (`BEGIN PUBLIC KEY`) |

**Hasil:** `boolean`. `false` bila verifikasi gagal.

{% hint style="warning" %}
* Urutan argumen: **signature dulu**, baru message.
* Base64 atau PEM yang tidak valid **melempar exception**, bukan `false`.
* Pesan harus identik byte demi byte.
{% endhint %}

```js
$GT.sha256_verify(signature, 'pesan', publicKeyPem);
$GT.sha256_verify(signature, 'pesan diubah', publicKeyPem); // false
try { $GT.sha256_verify('%%%', 'pesan', publicKeyPem); }
catch (err) { $GT.log(String(err)); }
```

## JWT RS256

### `$GT.jwtrs256_signing`

```js
$GT.jwtrs256_signing(claims, privateKeyPem)
```

| Parameter       | Keterangan                                                   |
| --------------- | ------------------------------------------------------------ |
| `claims`        | **Object** claim JWT (bukan string JSON)                     |
| `privateKeyPem` | PEM RSA privat yang diterima parser JWT (PKCS#1 atau PKCS#8) |

**Hasil:** token JWT bertanda tangan RS256.

Claim waktu (`exp`, `iat`) memakai Unix timestamp dalam **detik**. Method ini **tidak** menambahkan `exp`, `iss`, atau `aud` otomatis.

```js
$GT.jwtrs256_signing({sub: 'user-7'}, privateKeyPem);

const now = Math.floor(Date.now() / 1000);
const token = $GT.jwtrs256_signing({
  sub: 'user-7', iat: now, exp: now + 300,
  iss: 'example-app', aud: 'example-api'
}, privateKeyPem);
```

### `$GT.jwtrs256_verify`

```js
$GT.jwtrs256_verify(token, publicKeyPem)
```

**Hasil:** **string JSON** claims, jadi gunakan `JSON.parse`. Kegagalan parse/validasi umumnya melempar exception.

{% hint style="warning" %}
* Menerima keluarga `SigningMethodRSA`, tidak membatasi tepat RS256.
* **Tidak ada** parameter expected issuer/audience. Periksa `iss`/`aud` sendiri di script.
* Validasi claim waktu mengikuti parser JWT.
* Cabang token tidak valid tanpa error menghasilkan `undefined`.
{% endhint %}

```js
const claimsText = $GT.jwtrs256_verify(token, publicKeyPem);
const claims = JSON.parse(claimsText);
if (claims.iss !== 'example-app') { throw 'issuer tidak sesuai'; }

try { $GT.jwtrs256_verify('bukan-token', publicKeyPem); }
catch (err) { $GT.log(String(err)); }
```
