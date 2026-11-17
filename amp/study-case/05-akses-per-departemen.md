# Kasus 5: Akses per departemen

## Skenario

Satu perusahaan punya dua departemen, **FINANCE** dan **HR**. Anda ingin FINANCE hanya mengakses `/db/query`, sedangkan HR hanya `/http/get`. Setiap departemen punya kuota harian dan IP yang dibatasi.

## Prasyarat

Dua service sudah ada dan endpoint-nya bertipe **`user`**:

* `/db/query` ([Kasus 2](02-database-sebagai-api.md))
* `/http/get` ([Kasus 1](01-proxy-http.md))

## Rancangan

```text
 GROUP: Perusahaan
  ├─ CLIENT: FINANCE   (kuota per hari, credential, whitelist IP)
  │      └─ grant service: /db/query
  └─ CLIENT: HR        (kuota per hari, credential, whitelist IP)
         └─ grant service: /http/get
```

| Client | Credential | Grant service |
|---|---|---|
| FINANCE | API Key | `/db/query` |
| HR | OAuth2 | `/http/get` |

## Langkah

### 1. Buat Group

| Field | Isi |
|---|---|
| Name | `Perusahaan` |

### 2. Buat Client

Lakukan dua kali.

| Field | FINANCE | HR |
|---|---|---|
| Group | `Perusahaan` | `Perusahaan` |
| Name | `FINANCE` | `HR` |
| Quota per day | Sesuai kebutuhan | Sesuai kebutuhan |

### 3. Buat Credential

| Pengaturan | Isi |
|---|---|
| Tipe | `Basic`, `API Key`, atau `OAuth2` |
| Whitelist IP | IP yang diizinkan memakai credential |
| Tanggal expired | Batas berlaku credential |

Satu client bisa punya beberapa credential, misalnya satu per aplikasi.

### 4. Atur Grant service

Untuk setiap client, whitelist endpoint yang boleh diakses:

* FINANCE → `/db/query`
* HR → `/http/get`

{% hint style="warning" %}
Hanya endpoint bertipe **user** yang muncul di grant service.
{% endhint %}

## Hasil

* FINANCE dapat memanggil `/db/query`, tetapi tidak diberi akses ke `/http/get`.
* HR dapat memanggil `/http/get`, tetapi tidak diberi akses ke `/db/query`.
* Kuota harian dan whitelist IP berlaku per client dan credential.

## Uji

1. Panggil `/db/query` memakai credential FINANCE. Request seharusnya berhasil.
2. Panggil `/http/get` memakai credential FINANCE. Request seharusnya ditolak, karena tidak di-grant.
3. Panggil dari IP yang tidak ada di whitelist. Request seharusnya ditolak.

{% hint style="info" %}
Respons penolakan dan kode statusnya belum dijelaskan di materi sumber. Cek perilaku sebenarnya di lingkungan Anda.
{% endhint %}

Konsep lengkap: [Consumer](../consumer.md).
