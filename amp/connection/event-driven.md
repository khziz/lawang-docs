---
description: "Connection Event-driven di AMP v3.0 untuk proses yang dipicu event."
---

# Connection Event-driven

**Event-driven** adalah pendekatan arsitektur di mana proses dalam suatu sistem berjalan berdasarkan event atau kejadian tertentu, bukan karena suatu service harus secara langsung meminta service lain untuk melakukan proses.

## Gambaran penggunaan

Dipakai bila proses perlu berjalan saat sebuah event terjadi, tanpa service lain memanggilnya secara langsung.

Dari script JavaScript, pesan ke backend event-driven dikirim dengan `$GT.publish`. Lihat [Mengirim pesan dan memanggil SAP](javascript/methods/panduan/kirim-pesan-dan-sap.md).

## Konfigurasi

{% hint style="info" %}
Materi sumber saat ini hanya menjelaskan **konsep** connection ini. Langkah pengisian field connection, endpoint, dan service belum tersedia, jadi bagian tersebut perlu dilengkapi setelah materinya ada.
{% endhint %}

Alur umumnya sama dengan connection lain: **Connection → Endpoint → Service**. Lihat [Konsep dasar](../konsep-dasar.md).
