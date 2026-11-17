# HTTP & respons

{% hint style="success" %}
Baru belajar? Baca dulu [panduan langkah demi langkah](../panduan/panggil-api-lain.md). Halaman ini adalah **referensi teknis** untuk dicari ulang.
{% endhint %}

Method untuk memanggil API eksternal (`request`, `get`, `post`, `put`, `patch`, `delete`) dan untuk menentukan respons endpoint gateway (`response`).

## Aturan umum method HTTP

Berlaku untuk semua method `$GT.request`, `get`, `delete`, `post`, `put`, `patch`. **Mode:** gateway dan scheduler.

**Bentuk hasil**

```js
{ code: 200, headers: { 'Content-Type': ['application/json'] }, body: '...' }
```

| Field | Tipe | Keterangan |
|---|---|---|
| `code` | number | Status HTTP |
| `headers` | object | Nilai tiap header berupa **array string** |
| `body` | string | Dapat tidak ada |

**Aturan penting**

* Method **tidak** melempar exception untuk status HTTP gagal (misalnya 404/500). Periksa `code` sendiri.
* Error transport (connection gagal dan sejenisnya) **melempar exception**.
* `headers` boleh berupa object bernilai string, atau string JSON yang ekuivalen.
* Timeout dalam **detik**. `0` atau nilai di atas `RequestTimeout` diganti nilai konfigurasi.
* Request `GET` dapat memakai cache sesuai `BackendCacheLifetime`.

{% hint style="info" %}
`body` yang berupa object/array diserialisasi menjadi JSON. Untuk mengirim teks mentah, gunakan string biasa atau `{_raw_: '...'}`. Isi `_raw_` yang kosong tidak diekstrak.
{% endhint %}

{% hint style="warning" %}
**Integer di slot body/headers dianggap timeout.** Contoh: `$GT.post(url, 5)` berarti POST tanpa body dengan timeout 5 detik. Slot berikutnya dapat menimpa nilai itu, jadi untuk kejelasan **taruh timeout di slot terakhir**.
{% endhint %}

## `$GT.request`

Request HTTP dengan method bebas.

```js
$GT.request(method, url, body?, headers?, timeout?)
```

| Parameter | Tipe | Keterangan |
|---|---|---|
| `method` | string | Dinormalisasi menjadi huruf besar |
| `url` | string | URL lengkap |
| `body` | object / array / string | Isi request |
| `headers` | object / string JSON | Header request |
| `timeout` | integer | Detik |

```js
const a = $GT.request('GET', 'https://api.example.com/items');
const b = $GT.request('POST', 'https://api.example.com/items',
  {name: 'A'}, {'Content-Type': 'application/json'}, 10);
const c = $GT.request('PUT', 'https://api.example.com/raw',
  {_raw_: '<item>A</item>'}, {'Content-Type': 'application/xml'}, 5);
const d = $GT.request('GET', 'https://api.example.com/items', 5);
const e = $GT.request('POST', 'https://api.example.com/items',
  '{"name":"A"}', '{"Content-Type":"application/json"}', 10);

if (a.code === 200 && a.body) { const data = JSON.parse(a.body); }
```

Body `{_raw_: "..."}` mengirim string mentah jika tidak kosong.

## `$GT.get` dan `$GT.delete`

Request tanpa body.

```js
$GT.get(url, headers?, timeout?)
$GT.delete(url, headers?, timeout?)
```

| Parameter | Tipe | Keterangan |
|---|---|---|
| `url` | string | URL lengkap |
| `headers` | object / string JSON / integer | Integer dianggap timeout singkat |
| `timeout` | integer | Detik |

```js
$GT.get('https://api.example.com/items/7');
$GT.get('https://api.example.com/items/7', 5);
$GT.get('https://api.example.com/items/7',
  {Accept: 'application/json'}, 10);
$GT.get('https://api.example.com/items/7',
  '{"Accept":"application/json"}', 10);

$GT.delete('https://api.example.com/items/7');
$GT.delete('https://api.example.com/items/7', 5);
$GT.delete('https://api.example.com/items/7',
  {Accept: 'application/json'}, 10);
```

## `$GT.post`, `$GT.put`, `$GT.patch`

Request dengan body. Ketiganya memiliki bentuk yang sama.

```js
$GT.post(url, body?, headers?, timeout?)
$GT.put(url, body?, headers?, timeout?)
$GT.patch(url, body?, headers?, timeout?)
```

| Parameter | Tipe | Keterangan |
|---|---|---|
| `url` | string | URL lengkap |
| `body` | object / array / string / integer | Object diserialisasi JSON. Integer dianggap timeout |
| `headers` | object / string JSON / integer | Integer dianggap timeout |
| `timeout` | integer | Detik |

```js
$GT.post('https://api.example.com/items');
$GT.post('https://api.example.com/items', {name: 'A'});
$GT.post('https://api.example.com/items', {name: 'A'},
  {'Content-Type': 'application/json'}, 10);
$GT.post('https://api.example.com/items', '{"name":"A"}',
  '{"Content-Type":"application/json"}', 5);
$GT.post('https://api.example.com/raw', {_raw_: 'a=1&b=2'},
  {'Content-Type': 'application/x-www-form-urlencoded'}, 5);
$GT.post('https://api.example.com/items', 5); // tanpa body, timeout 5
```

`$GT.put` dan `$GT.patch` dipanggil dengan cara yang persis sama.

## `$GT.response`

Menentukan respons endpoint. **Mode:** gateway saja.

```js
$GT.response(body, code?, contentTypeOrHeaders?, headers?)
```

| Parameter | Tipe | Keterangan |
|---|---|---|
| `body` | any | Object/array diserialisasi JSON, nilai lain menjadi string |
| `code` | number | Status HTTP, default `200` |
| argumen ke-3 | string **atau** object | String = content type, object = header |
| argumen ke-4 | object | Header, dipakai bila argumen ke-3 bukan object |

**Hasil:** object internal `{body: string, code, contentType?, headers?: string JSON}`.

{% hint style="info" %}
* Jadikan pemanggilan ini **ekspresi terakhir** pada script biasa, atau `return` pada default export modul.
* Jika argumen ketiga berupa object, argumen keempat **diabaikan**.
* Header respons harus memetakan nama ke **string**.
{% endhint %}

```js
$GT.response({ok: true});
$GT.response({id: 7}, 201);
$GT.response('Halo', 200, 'text/plain');
$GT.response({ok: true}, 200, {'X-Trace': 'demo'});
$GT.response({ok: true}, 200, 'application/json',
  {'X-Trace': 'demo', 'Cache-Control': 'no-store'});
$GT.response('', 204);
```
