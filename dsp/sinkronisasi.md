---
description: "Direct, Upsync, Downsync, serta metode insert dan upsert."
---

# Sinkronisasi data

DSP menyediakan tiga cara sinkronisasi, semuanya lewat fungsi `$GT.sync`.

| Fitur | Arah | Keterangan |
|---|---|---|
| **Direct** | database → database | Antar-database langsung |
| **Upsync** | agent → database | Dari agent (misalnya kantor cabang) ke database pusat |
| **Downsync** | database → agent | Dari database pusat ke agent |

Masing-masing bisa memakai metode **insert** atau **upsert**.

## Insert atau upsert?

| Metode | Kapan | Cara |
|---|---|---|
| **Insert** | Menambah data ke tabel tujuan | **Tanpa** `identityFields` |
| **Upsert** | Perbarui bila sudah ada, tambah bila belum | **Dengan** `identityFields` (kolom penentu baris yang sama) |

## Struktur dasar `$GT.sync`

```js
$GT.sync(
  { /* sumber: connection, query, ... */ },
  { /* tujuan: connection, table, ... */ },
  { /* opsi: worker, batchSize */ }
);
```

Opsi:

| Opsi | Bawaan | Batas |
|---|---|---|
| `worker` | jumlah CPU | 1 sampai 256 |
| `batchSize` | 1000 | 1 sampai 10000 |

## Direct

Sinkronisasi dari database ke database.

**Insert:**

```js
$GT.sync({
  connection: "DB-SOURCE",
  query: "SELECT * FROM table-source"
}, {
  connection: "DB-TARGET",
  table: "table-target"
}, {"worker": 4, "batchSize": 1000});
```

**Upsert:**

```js
$GT.sync({
  connection: "DB-SOURCE",
  query: "SELECT * FROM table-source"
}, {
  connection: "DB-TARGET",
  table: "table-target",
  identityFields: ["field"]
}, {"worker": 4, "batchSize": 1000});
```

## Upsync (agent → database)

Tambahkan `agent` pada **sumber**.

**Insert:**

```js
$GT.sync({
  agent: 'agentName',
  connection: "DB-SOURCE",
  query: "SELECT * FROM table-source"
}, {
  connection: "DB-TARGET",
  table: "table-target"
}, {"worker": 4, "batchSize": 1000});
```

**Upsert:** tambahkan `identityFields: ["field"]` pada tujuan, seperti contoh Direct.

## Downsync (database → agent)

Tambahkan `agent` pada **tujuan**.

**Insert:**

```js
$GT.sync({
  connection: "DB-SOURCE",
  query: "SELECT * FROM table-source"
}, {
  agent: 'agentName',
  connection: "DB-TARGET",
  table: "table-target"
}, {"worker": 4, "batchSize": 1000});
```

**Upsert:** tambahkan `identityFields: ["field"]` pada tujuan.

{% hint style="warning" %}
Gunakan `agent` di **satu sisi saja**: di sumber untuk Upsync, di tujuan untuk Downsync. Mengisi agent di kedua sisi bukan kontrak relay.
{% endhint %}

{% hint style="info" %}
Contoh Downsync pada slide materi menaruh `agent` di sumber, sama seperti Upsync. Di sini kami menaruhnya di **tujuan**, sesuai dokumentasi fungsi `$GT.sync`. Mohon dicek ulang.
{% endhint %}

## Sinkronisasi dari SAP

Untuk menyalin tabel keluaran SAP ke database, gunakan `$GT.rfc_sync`. Lihat [Mengatur job di scheduler](../amp/connection/javascript/methods/panduan/job-scheduler.md#menyinkronkan-tabel-sap-rfc_sync).

**Referensi lengkap:** [Scheduler & sinkronisasi](../amp/connection/javascript/methods/referensi/scheduler.md)

## Study case

Contoh lengkap setiap arah sinkronisasi ada di [Study Case DSP](study-case/README.md).
