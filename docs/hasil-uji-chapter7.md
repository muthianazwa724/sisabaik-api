# Laporan Hasil Pengujian - Chapter 7 REST API SisaBaik

- **Tanggal Pengujian**: Oktober 2026
- **Database Target**: `sisabaik_dev` (PostgreSQL Lokal)
- **Lingkungan**: Node.js v22.x, Express 5.x

---

## Matriks Hasil Pengujian Aktual (Langkah 17)

| Skenario Pengujian | Ekspektasi | Status / Code HTTP | Hasil Aktual & Efek Database |
| :--- | :--- | :--- | :--- |
| **Health Check (`GET /api/health`)** | 200 OK | `200 OK` | Database terhubung (`connected`). |
| **GET Koleksi Penawaran** | 200 OK | `200 OK` | Data diambil langsung dari tabel PostgreSQL `penawaran`. |
| **ID Penawaran Bukan Angka (`/api/penawaran/abc`)** | 400 Bad Request | `400 VALIDATION_ERROR` | Ditolak helper validasi tanpa memicu SQL error. |
| **ID Valid Tidak Ada (`/api/penawaran/999999`)** | 404 Not Found | `404 OFFER_NOT_FOUND` | Tidak ada resource yang dihasilkan. |
| **POST Penawaran Valid** | 201 Created | `201 Created` | Header `Location` terbentuk, row baru masuk ke database. |
| **Harga Penawaran > Harga Normal** | 400 Bad Request | `400 VALIDATION_ERROR` | Ditolak validasi, tidak terjadi `INSERT`. |
| **PUT Lengkap** | 200 OK | `200 OK` | Atribut diperbarui, ID & penyedia asli tetap dipertahankan. |
| **PUT Tidak Lengkap** | 400 Bad Request | `400 VALIDATION_ERROR` | Data lama di database tidak mengalami perubahan. |
| **Pencarian & Filter Gabungan** | 200 OK | `200 OK` | Kueri SQL terparameterisasi menyaring data dengan benar. |
| **Halaman di Luar Batas (`page=2` saat data 1)** | 200 OK | `200 OK` | `data: []`, metadata `jumlah` tetap konsisten. |
| **Input SQL Injection (`q=" OR 1=1 --")** | 200 OK | `200 OK` | Diperlakukan sebagai teks biasa, `jumlah: 0`. |
| **DELETE Penawaran Tanpa Pesanan** | 204 No Content | `204 No Content` | Row terhapus dari database (`GET` berikutnya `404`). |
| **DELETE Penawaran Bersejarah** | 409 Conflict | `409 REFERENCE_CONFLICT` | Ditolak oleh constraint foreign key, riwayat aman. |
| **POST Pesanan Lintas Penyedia** | 409 Conflict | `409 MIXED_PROVIDER` | Transaksi dibatalkan (`ROLLBACK`), stok aman. |
| **POST Pesanan Stok Kurang** | 409 Conflict | `409 INSUFFICIENT_STOCK` | Transaksi dibatalkan (`ROLLBACK`), stok aman. |
| **POST Pesanan Valid** | 201 Created | `201 Created` | Row pesanan & detail pesanan terbentuk, stok berkurang. |
| **Update Harga Penawaran** | 200 OK | `200 OK` | Snapshot `harga_penawaran_satuan` pada detail pesanan lama tetap Rp 5.000. |
| **JSON Rusak / Content-Type / Payload > 20KB** | 400 / 415 / 413 | `400`, `415`, `413` | Penanganan middleware error bekerja, data aman. |
| **Uji Persistensi (Server Restart)** | Data Tetap Ada | `200 OK` | Pesanan ID 4 tetap dapat diakses setelah server di-restart. |
| **Uji Konkurensi (2 Request Bersamaan)** | 1x 201, 1x 409 | `201` & `409` | Row lock `FOR UPDATE` berfungsi. Tepat 1 pesanan berhasil, stok akhir 0. |