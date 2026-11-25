# Istilah penting

| Istilah | Artinya |
|---|---|
| **Gateway** | Mode yang menangani request HTTP masuk dan meresponsnya |
| **Scheduler** | Mode yang menjalankan job terjadwal atau job yang dipanggil |
| **Job** | Satu pekerjaan di scheduler, dikenali lewat **kode job** |
| **`$GT`** | Object berisi semua method dan data bawaan Lawang |
| **Backend** | Layanan tujuan yang sudah didaftarkan: database, file, SAP, atau event |
| **Source / sumber** | **Nama** backend yang dipakai method, bukan alamat atau password-nya |
| **Layanan pusat** | Tempat konfigurasi connection, secret, job, dan backend disimpan |
| **Secret** | Kunci rahasia yang disimpan di layanan pusat. Script hanya memakai namanya |
| **Payload** | Isi (body) request atau data yang dikirim |
| **Timeout** | Batas waktu tunggu dalam **detik** |
| **`RequestTimeout`** | Batas waktu standar dari konfigurasi |
| **Exception** | Error yang dilempar. Ditangkap dengan `try/catch` |
| **Placeholder `?`** | Tanda di SQL yang diisi nilai dari array parameter, supaya aman |
| **Base64** | Cara menulis data biner (misalnya isi file) sebagai teks |
| **Upsert** | *Update* bila sudah ada, *insert* bila belum |
| **Identity / identitas** | Kolom yang menentukan "baris yang sama" (misalnya `id`) |
| **RFC** | Cara memanggil fungsi di sistem **SAP** |
| **HSM** | Perangkat keras khusus untuk menyimpan kunci enkripsi |
| **Agent** | Program Lawang di lokasi lain (misalnya cabang) untuk sinkronisasi |
| **Up-sync** | Sinkronisasi dari agent **ke** scheduler |
| **Down-sync** | Sinkronisasi dari scheduler **ke** agent |
| **Modul** | Script yang memakai `import`/`export default` |
| **Sobek** | Engine JavaScript yang menjalankan script |
