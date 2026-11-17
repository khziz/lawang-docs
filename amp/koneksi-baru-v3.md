# Koneksi baru di AMP v3.0

AMP v3.0 menambahkan beberapa jenis connection baru. Halaman ini menjelaskan **apa fungsi masing-masing**.

{% hint style="info" %}
Materi sumber saat ini menjelaskan konsep tiap koneksi. Langkah pengisian field untuk setiap jenis belum tercakup.
{% endhint %}

| Koneksi | Penjelasan |
|---|---|
| **gRPC** | gRPC (Google Remote Procedure Call) adalah framework komunikasi antar-service/aplikasi lewat jaringan, dengan cara memanggil fungsi di service lain seolah-olah fungsi itu ada di aplikasi sendiri |
| **GraphQL** | Query language dan runtime untuk API yang memungkinkan client meminta data yang benar-benar dibutuhkan, tidak lebih dan tidak kurang |
| **Load Balancer** | Mendistribusikan traffic/request dari client ke beberapa server agar beban tidak menumpuk pada satu server |
| **Proxy** | Perantara antara client dan server. Client tidak berkomunikasi langsung dengan server tujuan, tetapi request melewati proxy terlebih dahulu |
| **Asynchronous** | Mekanisme agar sistem otomatis mengirim HTTP request ke sistem lain ketika sebuah event terjadi |
| **Event-driven** | Pendekatan arsitektur di mana proses berjalan berdasarkan event atau kejadian tertentu, bukan karena satu service secara langsung meminta service lain untuk memproses |
| **SAP RFC** | Komunikasi AMP dengan SAP lewat RFC, memakai adapter (lihat di bawah) |

## Model komunikasi SAP RFC dan AMP

AMP **tidak berkomunikasi langsung** dengan SAP RFC. Komunikasi memakai **SAP RFC Adapter** sebagai jembatan protokol (*protocol bridge*).

```text
  AMP  ──HTTP──►  SAP RFC Adapter  ──RFC──►  SAP
  AMP  ◄─HTTP───  SAP RFC Adapter  ◄─RFC───  SAP
```

Tugas adapter:

1. Menerima HTTP request dari AMP.
2. Memetakan parameter request ke parameter SAP RFC.
3. Memanggil SAP Function Module lewat RFC.
4. Mengonversi response SAP menjadi HTTP response untuk AMP.

Dari skrip JavaScript, Anda bisa memanggil SAP dengan `$GT.rfc`. Lihat [Mengirim pesan dan memanggil SAP](javascript/panduan/kirim-pesan-dan-sap.md).
