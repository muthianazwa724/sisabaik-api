Developed by : Muthia Nazwa Marlina NIM.23367036

# SisaBaik API - Chapter 8

SisaBaik adalah proyek berkembang pada mata kuliah Pengembangan Web. Aplikasi ini dirancang untuk membantu penyedia memulihkan nilai stok makanan berlebih melalui penjualan dengan harga khusus atau penyaluran, sekaligus membantu masyarakat memperoleh makanan yang lebih terjangkau dan mengurangi limbah pangan. Pada Chapter 8 ini, sistem melengkapi fondasi akun dengan autentikasi (JWT) dan otorisasi berbasis peran (RBAC) serta peninjauan usaha penyedia.

## Capaian Chapter 8

- **Migrasi Basis Data**: Menambahkan kolom `kata_sandi_hash` (TEXT) dan `versi_token` (INTEGER) pada tabel `pengguna` melalui `database/07_auth.sql` tanpa menghapus data historis Chapter 6-7.
- **Pendaftaran Mandiri & Hash Sandi**: Mengimplementasikan registrasi mandiri untuk peran Pembeli (`POST /api/auth/registrasi`) dan Penyedia (`POST /api/auth/registrasi-penyedia`) dengan *hashing* kata sandi menggunakan `bcryptjs` (faktor kerja 12).
- **Peninjauan Usaha Penyedia**: Pendaftaran penyedia secara otomatis menghasilkan status usaha `menunggu`. Pengelola/operator meninjau dan menyetujui izin usaha via script CLI (`npm run penyedia:verifikasi <email>`).
- **Autentikasi JWT**: Penerbitan JSON Web Token (HS256) berdurasi 15 menit saat login (`POST /api/auth/login`) yang dikirim melalui header `Authorization: Bearer <token>`.
- **Otorisasi Server-Side (RBAC) & Kepemilikan Data**:
  - Middleware `wajibLogin`, `wajibPeran`, dan `wajibPenyediaTerverifikasi` membatasi akses endpoint terlindungi.
  - Penyedia terverifikasi hanya dapat mengelola penawaran miliknya (`GET /api/penawaran/milik-saya`, `PUT/DELETE /api/penawaran/:id`).
  - Pembeli hanya dapat memesan atas akun sendiri (`POST /api/pesanan`), dan pesanan dibatasi agar hanya dapat dibaca oleh pembeli pemesan atau penyedia penerima.
- **Pencabutan Sesi (Logout)**: Menambahkan mekanisme *logout* seluruh sesi (`POST /api/auth/logout`) dengan menaikkan `versi_token` pada database untuk membatalkan semua token lama.
- **Pengamanan Tambahan**: Menerapkan Helmet header keamanan, pembatas laju request (*rate-limiting*) terpisah untuk login dan registrasi, pengabaian `.env` dari Git, serta respon `401/403/404` yang tidak membocorkan data privat.
- **Pengujian Otomatis**: Lolos 44 pemeriksaan assertion HTTP dan pengujian kepemilikan data via `npm run auth:test`.

## Struktur Berkas Utama

- `server.js` : Berkas utama (*entry point*) untuk menjalankan server Express pada port lokal `3000`.
- `src/app.js` : Konfigurasi aplikasi Express, middleware keamanan (Helmet, rate limiter, logger, JSON limit 20kb, static files), serta pendaftaran rute.
- `src/config/auth.js` : Konfigurasi rahasia JWT (64-digit hex) dan parameter autentikasi.
- `src/lib/` :
  - `token.js` : Helper penerbitan (`sign`) dan verifikasi (`verify`) JWT HS256.
  - `input-auth.js` : Helper validasi skema input registrasi, login, dan validasi sesi.
  - `api.js` : Helper validasi umum dan pemetaan kelas `HttpError`.
- `src/repositories/` :
  - `pengguna.js` : Akses data SQL langsung ke tabel `pengguna` dan `penyedia` (termasuk fungsi registrasi transaksi atomik).
  - `penawaran.js` : Akses data SQL `penawaran` dengan kondisi penguncian kepemilikan `penyedia_id`.
- `src/services/` :
  - `auth.js` : Logika bisnis autentikasi (pencocokan hash bcrypt, pencegahan timing-attack dengan `hashDummy`, penerbitan token).
  - `pesanan.js` : Logika bisnis transaksi pemesanan berbasis `pembeli_id` dari token terautentikasi.
- `src/middleware/` :
  - `auth.js` : Middleware verifikasi JWT (`wajibLogin`), pengecekan peran (`wajibPeran`), dan otorisasi penyedia (`wajibPenyediaTerverifikasi`).
  - `batas-auth.js` : Rate-limiter terpisah untuk login (`batasAuth`) dan pendaftaran (`batasRegistrasi`).
- `src/routes/` : Pendaftaran endpoint HTTP untuk `/api/auth`, `/api/penawaran`, dan `/api/pesanan`.
- `public/` : Halaman penguji latihan autentikasi (`auth.html`, `auth.css`, dan `auth.js`) menggunakan penyimpanan token di memori.
- `scripts/` :
  - `verifikasi-penyedia.js` : Script CLI operator untuk meninjau/menyetujui status penyedia (`npm run penyedia:verifikasi`).
  - `uji-auth.js` : Script pengujian otomatis untuk alur autentikasi, otorisasi, dan kepemilikan (`npm run auth:test`).
- `docs/` : Spesifikasi kontrak API (`API.md`) dan laporan hasil pengujian aktual (`hasil-uji-chapter8.md`).

## Prasyarat & Konfigurasi Lingkungan

1. **Node.js**: Minimum v22.0.0.
2. **PostgreSQL**: Terpasang lokal dengan database `sisabaik_dev` dan migrasi `database/07_auth.sql` yang sudah diterapkan.
3. **Berkas Environment (`.env`)**:
   Salin `.env.example` menjadi `.env` di root proyek:
   ```env
   PORT=3000
   PGHOST=localhost
   PGPORT=5432
   PGDATABASE=sisabaik_dev
   PGUSER=postgres
   PGPASSWORD=ganti_dengan_password_lokal
   JWT_SECRET=ganti_dengan_64_digit_hex_acak