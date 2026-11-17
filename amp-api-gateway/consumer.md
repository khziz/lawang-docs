# Consumer

**Consumer** adalah pihak yang memanggil API Anda. Bagian ini mengatur _siapa_ boleh memanggil _apa_.

## Alur konfigurasi

```
 ENDPOINT ──► SERVICE        (sisi API: apa yang disajikan)

 GROUP ──► CLIENT ──► CREDENTIAL ──► GRANT SERVICE   (sisi consumer: siapa yang boleh)
```

Urutan membuat sisi consumer: **Group → Client → Credential → Grant service**.

## 1. Group

Kelompok besar, misalnya satu perusahaan atau departemen.

| Field    | Isi                     |
| -------- | ----------------------- |
| **Name** | Bebas, sesuai kebutuhan |

## 2. Client

Satu group dapat memiliki **lebih dari satu** client.

| Field             | Isi                                      |
| ----------------- | ---------------------------------------- |
| **Group**         | Pilih group yang sudah dibuat            |
| **Name**          | Bebas, misalnya `FINANCE`                |
| **Quota per day** | Batas jumlah request per hari (opsional) |

## 3. Credential

Kunci yang dipakai client untuk memanggil API.

| Pengaturan          | Isi                               |
| ------------------- | --------------------------------- |
| **Tipe**            | `Basic`, `API Key`, atau `OAuth2` |
| **Whitelist IP**    | Daftar IP yang diizinkan          |
| **Tanggal expired** | Batas berlaku credential          |

Satu client dapat memiliki beberapa credential.

## 4. Grant service

Berfungsi untuk **mem-whitelist endpoint** pada client tertentu.

{% hint style="warning" %}
Hanya endpoint bertipe **user** yang bisa dimasukkan ke grant service.
{% endhint %}

## Struktur group, client, dan credential

```
 GROUP 1
  ├─ client A
  │    ├─ credential-1
  │    ├─ credential-2
  │    └─ credential-3
  └─ client B
       ├─ credential-1
       ├─ credential-2
       └─ credential-3
```

Dalam satu group ada beberapa client, dan setiap client punya beberapa credential.

## Level akses endpoint

Saat membuat endpoint, Anda memilih satu dari tiga **access level**:

| Level     | Keterangan                                                 |
| --------- | ---------------------------------------------------------- |
| `public`  | Pilihan level akses                                        |
| `private` | Pilihan level akses                                        |
| `user`    | Endpoint yang dapat di-grant ke client lewat grant service |

{% hint style="info" %}
Materi sumber hanya menyebut ketiga level ini dan aturan grant untuk level `user`. Definisi rinci `public` dan `private` belum dijelaskan, jadi lengkapi bagian ini setelah dikonfirmasi tim.
{% endhint %}
