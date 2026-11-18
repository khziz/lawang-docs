---
description: "Connection gRPC di AMP v3.0 untuk komunikasi antar-service."
---

# Connection gRPC

**gRPC** (Google Remote Procedure Call) adalah framework untuk membuat komunikasi antar-service atau aplikasi melalui jaringan, dengan cara memanggil fungsi di service lain seolah-olah fungsi tersebut ada di aplikasi sendiri.

## Gambaran penggunaan

Dipakai bila layanan tujuan berkomunikasi dengan gRPC. Core AMP sendiri juga menyediakan alamat gRPC, yang tampil di halaman [Statistics server](../settings.md#statistics-server).

## Konfigurasi

{% hint style="info" %}
Materi sumber saat ini hanya menjelaskan **konsep** connection ini. Langkah pengisian field connection, endpoint, dan service belum tersedia, jadi bagian tersebut perlu dilengkapi setelah materinya ada.
{% endhint %}

Alur umumnya sama dengan connection lain: **Connection → Endpoint → Service**. Lihat [Konsep dasar](../konsep-dasar.md).
