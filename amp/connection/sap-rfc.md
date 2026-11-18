---
description: "Connection SAP RFC di AMP v3.0 dan model komunikasi AMP dengan SAP lewat SAP RFC Adapter."
---

# Connection SAP RFC

**SAP RFC** menghubungkan AMP dengan SAP. AMP **tidak berkomunikasi langsung** dengan SAP, melainkan lewat **SAP RFC Adapter** yang berperan sebagai *protocol bridge*.

## Model komunikasi

```text
  AMP  ──HTTP──►  SAP RFC Adapter  ──RFC──►  SAP
  AMP  ◄─HTTP───  SAP RFC Adapter  ◄─RFC───  SAP
```

| Pihak | Protokol |
|---|---|
| AMP ↔ Adapter | HTTP |
| Adapter ↔ SAP | RFC |

Adapter diperlukan karena AMP tidak berkomunikasi langsung dengan SAP.

## Tugas adapter

1. Menerima HTTP request dari AMP.
2. Memetakan parameter request ke parameter SAP RFC.
3. Memanggil SAP Function Module lewat RFC.
4. Mengonversi response SAP menjadi HTTP response untuk AMP.

## Memanggil SAP dari script

Dari script JavaScript, gunakan method `$GT.rfc` (satu kali panggil) atau `$GT.rfc_sync` (sinkronisasi tabel SAP ke database di DSP):

* [Mengirim pesan dan memanggil SAP](javascript/methods/panduan/kirim-pesan-dan-sap.md)
* [`$GT.rfc_sync` di DSP](../../dsp/study-case/05-sync-sap.md)

## Konfigurasi

{% hint style="info" %}
Materi sumber saat ini hanya menjelaskan **model komunikasi**. Langkah pengisian field connection, endpoint, dan service belum tersedia, jadi bagian tersebut perlu dilengkapi setelah materinya ada.
{% endhint %}
