Developed by : Muthia Nazwa Marlina NIM.23367036

# SisaBaik API - Chapter 7

SisaBaik adalah proyek berkembang pada mata kuliah Pengembangan Web. Aplikasi ini dirancang untuk membantu penyedia memulihkan nilai stok makanan berlebih melalui penjualan dengan harga khusus atau penyaluran, sekaligus membantu masyarakat memperoleh makanan yang lebih terjangkau dan mengurangi limbah pangan.

## Capaian Chapter 7

- Mengintegrasikan server Express.js dengan basis data PostgreSQL (`sisabaik_dev`) menggantikan penyimpanan array sementara.
- Membangun endpoint REST API lengkap (GET, POST, PUT, DELETE) untuk manajemen penawaran dan pesanan.
- Menerapkan kueri terparameterisasi (`$1`, `$2`) untuk mencegah celah keamanan SQL Injection.
- Menerapkan validasi input berlapis (path parameter, query parameter, JSON body, tipe data, dan nilai numerik).
- Menerapkan transaksi atomik pada pembuatan pesanan lengkap dengan penguncian stok (`FOR UPDATE`), *snapshot* harga, dan mekanisme *rollback*.
- Menangani kesalahan (*error handling*) secara terpusat dengan pemetaan kode status HTTP dan format respon JSON yang konsisten.
- Menerapkan fitur pencarian, filter gabungan, pengurutan tetap, serta paginasi (`page`, `limit`, `offset`) dengan metadata lengkap.
- Menguji skenario positif, negatif, batas konkurensi stok (*row lock*), serta membuktikan persistensi data setelah server di-restart.

## Struktur Berkas Utama

- `server.js` : Berkas utama (*entry point*) untuk menjalankan server Express pada port lokal `3000`.
- `src/app.js` : Konfigurasi aplikasi Express, middleware (logger, JSON parser, static files, error handler), dan pendaftaran rute.
- `src/routes/` : Pendaftaran endpoint HTTP untuk `/api/penawaran` dan `/api/pesanan`.
- `src/controllers/` : Penanganan logika input/output HTTP dan panggilan ke repositori/layanan.
- `src/services/pesanan.js` : Logika bisnis transaksi pemesanan (pemeriksaan pembeli, *lock* penawaran, *snapshot* harga, dan kalkulasi total).
- `src/repositories/penawaran.js` : Akses data SQL langsung ke tabel `penawaran` di PostgreSQL.
- `src/lib/api.js` : *Helper* validasi input, batas halaman, dan pemetaan kelas `HttpError`.
- `src/db/` : Konfigurasi *connection pool* PostgreSQL dan *helper* transaksi (`BEGIN`, `COMMIT`, `ROLLBACK`).
- `public/` : Halaman penguji ringan (`index.html` dan `app.js`) untuk simulasi pemesanan via browser.
- `scripts/` : Script pengujian otomatis untuk API (`uji-api.js`) dan uji konkurensi stok (`uji-konkurensi.js`).
- `docs/` : Dokumentasi spesifikasi kontrak API (`API.md`) dan matriks laporan hasil pengujian (`hasil-uji-chapter7.md`).

## Prasyarat & Konfigurasi Lingkungan

1. **Node.js**: Minimum v22.0.0.
2. **PostgreSQL**: Terpasang lokal dengan database `sisabaik_dev` dan 6 tabel aktif hasil Chapter 6 (`pengguna`, `penyedia`, `penawaran`, `pesanan`, `detail_pesanan`, `pengambilan`).
3. **Berkas Environment (`.env`)**:
   Salin `.env.example` menjadi `.env` di root proyek:
   ```env
   PORT=3000
   PGHOST=localhost
   PGPORT=5432
   PGDATABASE=sisabaik_dev
   PGUSER=postgres
   PGPASSWORD=ganti_dengan_password_lokal