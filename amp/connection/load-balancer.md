---
description: "Connection Load Balancer di AMP v3.0 untuk membagi traffic ke beberapa server."
---

# Connection Load Balancer

**Load Balancer** berfungsi mendistribusikan traffic atau request dari client ke beberapa server agar beban tidak menumpuk pada satu server.

## Gambaran penggunaan

Dipakai bila satu layanan dijalankan di beberapa server dan request perlu dibagi di antara mereka.

{% hint style="warning" %}
Jangan tertukar dengan **Load Balancer** pada [arsitektur AMP](../arsitektur.md#komponen-amp), yang membagi traffic ke **Core**. Halaman ini membahas **connection** Load Balancer untuk layanan tujuan.
{% endhint %}

## Konfigurasi

{% hint style="info" %}
Materi sumber saat ini hanya menjelaskan **konsep** connection ini. Langkah pengisian field connection, endpoint, dan service belum tersedia, jadi bagian tersebut perlu dilengkapi setelah materinya ada.
{% endhint %}

Alur umumnya sama dengan connection lain: **Connection → Endpoint → Service**. Lihat [Konsep dasar](../konsep-dasar.md).
