# Membuat server, jadwal, dan job

Sebuah job butuh dua hal lebih dulu: **server** (tempat job jalan) dan **schedule** (jadwalnya).

```text
 Create Server ─┐
                ├──► Create Job
 Create Schedule┘
```

## Langkah 1: Buat server sebagai scheduler

1. Masuk ke menu **Server**, lalu klik **Create new**.
2. Isi **nama** dan **host**.
3. Aktifkan toggle **Set as scheduler**.
4. Klik **Submit**.

## Langkah 2: Buat schedule

1. Isi **name** dan **description**.
2. Atur **cron schedule**. Anda bisa memilih opsi yang tersedia atau menulis **custom**.
3. Anda bisa **menguji sintaks** dulu, atau langsung **Submit**.

## Langkah 3: Buat job

1. Isi **name**.
2. Atur **timeout** (opsional).
3. Pilih **schedule** yang sudah dibuat.
4. Pilih **server** yang sudah dibuat.
5. Tulis **script**, yaitu aksi yang ingin dilakukan.
6. Klik **Submit**.

## Setelah itu

Job berjalan pada jadwalnya. Anda juga bisa memicunya **manual**, atau memanggilnya dari job lain dengan `$GT.run`.

Contoh skrip untuk langkah 5 ada di [Menulis script job](script-job.md).
