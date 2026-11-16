# Cara membaca dokumen & aturan runtime

## Cakupan

Dokumen ini mencakup:

* Seluruh 40 nama pada `configs.FunctionsMap`
* Dua fungsi scheduler tambahan: `run` dan `parallel`
* Helper global `atob`, `btoa`, dan `console.log`
* Fungsi global bawaan engine

Constructor bawaan diringkas di lampiran. Metode standar seperti `Array.prototype.map` atau `Math.sin` **bukan** API khusus Lawang, jadi tidak dibahas satu per satu.

## Membaca sintaks

* Tanda `?` berarti parameter **opsional**. Jangan menulis tanda `?` saat memanggil fungsi.
* Setiap blok "Variasi pemanggilan" berisi **alternatif**, bukan urutan yang harus dijalankan sekaligus.
* Contoh yang menulis ke database, menyimpan file, `publish`, atau `sync` dapat mengubah sistem tujuan.

## Aturan runtime

### 1. Cara fungsi `$GT` dimuat

Fungsi `$GT` dimuat berdasarkan deteksi referensi **literal** `$GT.nama` di sumber skrip dan dependensi modul.

{% hint style="warning" %}
Gunakan bentuk titik (`$GT.query(...)`). Bentuk dinamis seperti `$GT[nama]`, atau destructuring tanpa referensi literal, dapat membuat fungsi **tidak dimuat**.
{% endhint %}

`run` dan `parallel` dipasang langsung oleh scheduler, jadi tidak terkena aturan ini.

### 2. Konfigurasi berasal dari layanan pusat

Koneksi, secret, job, dan backend diambil dari layanan pusat lewat gRPC. Nama pada contoh bukan definisi koneksi lokal.

### 3. Gateway dan scheduler terpisah

* `response` hanya untuk **gateway**.
* `sync`, `rfc_sync`, `run`, `parallel` hanya untuk **scheduler**.

### 4. Timeout

* Satuan timeout adalah **detik**.
* Untuk HTTP, database, dan RFC: timeout `0` atau lebih besar dari `RequestTimeout` diganti dengan nilai konfigurasi.
* Timeout negatif **bukan** cara aman menonaktifkan batas. Context induk tetap berlaku dan sebagian helper hanya memasang timeout jika positif. Pilih nilai positif.

### 5. Sinkron, bukan Promise

Semua helper sinkron dari sisi skrip dan **tidak** mengembalikan Promise. Runtime baru dibuat setiap eksekusi.

Sobek bukan Node.js atau browser. Hal-hal berikut **tidak** diinjeksi Lawang:

`fetch`, `setTimeout`, `setInterval`, `require`, `process`, `Buffer`, `window`, `document`

### 6. Error umumnya berupa string

Error biasanya dilempar sebagai **string**, bukan selalu `Error` JavaScript. Gunakan `String(err)` atau periksa tipenya.

Error RFC dapat berupa object terstruktur dengan field: `type`, `category`, `operation`, `summary`, `message`, `code`, `key`, `suggestion`, `abap_class`, `abap_type`, `abap_number`, `retryable` (sebagian opsional).

```js
try {
  const rows = $GT.query('db_main', 'SELECT id FROM items', [], 5);
  $GT.log({count: rows.length});
} catch (err) {
  $GT.log(typeof err === 'object' ? err : String(err));
}
```

### 7. Modul

* Mendukung `import` statis dari lokasi yang diizinkan runtime. **Dynamic import ditolak.**
* Default export wajib berupa function. Tanpa default export, runtime dapat mengambil respons yang ditangkap `$GT.response`.
* Batas graph modul: **1 MiB** per source, **8 MiB** total, **128** modul.
* Tidak ada event loop Node.js.
