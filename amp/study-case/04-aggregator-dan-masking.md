# Kasus 4: Menggabungkan API dan menyamarkan data

## Skenario

Aplikasi mobile butuh data customer dari API internal, tetapi nomor telepon tidak boleh tampil utuh. Anda membuat endpoint JavaScript yang memanggil API tersebut, lalu **menyamarkan** nomor telepon sebelum dikirim.

## Komponen

| Komponen | Nilai contoh |
|---|---|
| Connection JavaScript | `js-main` |
| Endpoint | `/customer` (GET) |
| Service | `/api/customer` |

## Script

```js
export default function () {
  const q = $GT.queries ? JSON.parse($GT.queries) : {};
  const id = q.id ? q.id[0] : null;

  if (!id) {
    return $GT.response({message: 'id wajib diisi'}, 400);
  }

  let res;
  try {
    res = $GT.get('https://api.example.com/customers/' + encodeURIComponent(id),
      {Accept: 'application/json'}, 10);
  } catch (err) {
    $GT.log(String(err));
    return $GT.response({message: 'Layanan tujuan tidak dapat dihubungi'}, 502);
  }

  if (res.code === 404) {
    return $GT.response({message: 'Customer tidak ditemukan'}, 404);
  }
  if (res.code !== 200 || !res.body) {
    $GT.log({upstream_status: res.code});
    return $GT.response({message: 'Layanan tujuan bermasalah'}, 502);
  }

  const c = JSON.parse(res.body);
  const phone = String(c.phone || '');
  // tampilkan 4 digit terakhir saja
  const maskedPhone = phone.length > 4
    ? $GT.mask_prefix(phone, '*', phone.length - 4)
    : phone;

  return $GT.response({id: c.id, name: c.name, phone: maskedPhone}, 200);
}
```

## Cara kerja

1. Baca `id` dari query string, tolak bila kosong (`400`).
2. Panggil API internal dengan `$GT.get` dan timeout 10 detik.
3. **Kegagalan connection** melempar exception, jadi dibungkus `try/catch`.
4. **Status HTTP** tidak melempar exception, jadi `res.code` dicek manual.
5. Samarkan telepon dengan `$GT.mask_prefix`, sisakan 4 digit terakhir.

## Hasil

Request `?id=42` menghasilkan response seperti:

```json
{"id": 42, "name": "Budi", "phone": "********1234"}
```

## Catatan

* Masking bekerja per **byte**. Gunakan teks ASCII agar karakter tidak terpotong.
* Jika `n` masking sama dengan atau lebih besar dari panjang teks, nilai asli dikembalikan tanpa disamarkan. Karena itu script menjaga `phone.length > 4`.
* Method terkait: [`$GT.get`](../connection/javascript/methods/referensi/http.md), [`$GT.mask_prefix`](../connection/javascript/methods/referensi/masking-transformasi.md).
