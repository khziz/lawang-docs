# Memanggil API lain

**Mode:** gateway dan scheduler. **Anda akan belajar:** mengirim request HTTP dan membaca jawabannya.

## Langkah 1: Pilih fungsi

| Tujuan | Fungsi |
|---|---|
| Mengambil data | `$GT.get(url, headers?, timeout?)` |
| Mengirim data baru | `$GT.post(url, body?, headers?, timeout?)` |
| Mengganti data | `$GT.put(...)` |
| Mengubah sebagian data | `$GT.patch(...)` |
| Menghapus | `$GT.delete(url, headers?, timeout?)` |
| Method lain / dinamis | `$GT.request(method, url, body?, headers?, timeout?)` |

## Langkah 2: Kirim request

### GET

```js
const res = $GT.get('https://api.example.com/items/7',
  {Accept: 'application/json'}, 10);
```

### POST dengan JSON

```js
const res = $GT.post('https://api.example.com/items',
  {name: 'A'},
  {'Content-Type': 'application/json'}, 10);
```

Object pada body otomatis diubah menjadi JSON.

### Body teks mentah (XML, form, dsb.)

```js
$GT.post('https://api.example.com/raw',
  {_raw_: 'a=1&b=2'},
  {'Content-Type': 'application/x-www-form-urlencoded'}, 5);
```

Atau kirim string biasa: `'{"name":"A"}'`.

## Langkah 3: Baca jawabannya

Jawaban berbentuk object:

```js
{ code: 200, headers: {'Content-Type': ['application/json']}, body: '{"id":7}' }
```

* `code`: status HTTP (angka).
* `headers`: nilai tiap header berupa **array**.
* `body`: **teks**, dan bisa tidak ada.

```js
if (res.code === 200 && res.body) {
  const data = JSON.parse(res.body);
  $GT.log(data);
} else {
  $GT.log({gagal: res.code});
}
```

## Langkah 4: Tangani kegagalan

* Status `404`/`500` **tidak** melempar error. Periksa `res.code`.
* Koneksi gagal **melempar exception**. Gunakan `try/catch`.

```js
try {
  const res = $GT.get('https://api.example.com/items', 5);
  if (res.code !== 200) { throw 'API menjawab ' + res.code; }
} catch (err) {
  $GT.log(String(err));
}
```

## Hal yang perlu diingat

* Timeout adalah **detik**. Taruh di argumen **terakhir**.
* Integer di slot body atau header dianggap timeout. Contoh `$GT.post(url, 5)` = POST tanpa body, timeout 5 detik.
* Request `GET` dapat di-cache sesuai `BackendCacheLifetime`.

**Referensi:** [HTTP & respons](../referensi/http.md)
