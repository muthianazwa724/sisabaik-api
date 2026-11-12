# SisaBaik – Chapter 10: Integrasi Full Stack

**Developed by:** Muthia Nazwa Marlina  
**NIM:** 23367036

SisaBaik merupakan proyek mata kuliah Pengembangan Web yang dikembangkan untuk membantu penyedia memulihkan nilai stok makanan berlebih melalui penjualan dengan harga khusus atau penyaluran. Aplikasi ini juga membantu masyarakat memperoleh makanan yang lebih terjangkau dan mengurangi limbah pangan.

Pada Chapter 10, pengembangan difokuskan pada integrasi frontend Vue 3 dengan backend Express dan database PostgreSQL, termasuk autentikasi pengguna, pengelolaan penawaran, pemesanan, serta dashboard.

## Capaian Chapter 9

### 1. Vue 3 Composition API
Menggunakan Composition API untuk mengelola data dan interaksi aplikasi, meliputi:
- `ref` untuk menyimpan state.
- `computed` untuk menghasilkan data turunan.
- `props` untuk mengirim data antarkomponen.
- `emits` untuk mengirim event dari komponen anak ke induk.

### 2. Komponen Reusable
Membuat komponen yang dapat digunakan kembali:
- `KartuPenawaran.vue`
- `FilterPenawaran.vue`
- `FormPenawaran.vue`
- `Ikon.vue`

### 3. Katalog dan Filter Penawaran
- Menampilkan daftar penawaran makanan.
- Pencarian berdasarkan nama makanan atau penyedia.
- Filter berdasarkan kategori makanan.
- Menampilkan pesan ketika hasil pencarian kosong.

### 4. Mode Pembeli dan Penyedia
- Mode Pembeli menampilkan katalog makanan yang tersedia.
- Mode Penyedia menampilkan daftar penawaran milik penyedia latihan.
- Menghitung jumlah penawaran dan stok menggunakan `computed`.

### 5. Formulir dan Validasi
- Menambahkan penawaran makanan melalui formulir.
- Memvalidasi nama, kategori, harga normal, harga penawaran, stok, status, dan waktu berakhir.
- Harga penawaran sebesar 0 diperbolehkan untuk penawaran gratis.
- Menolak data yang tidak memenuhi ketentuan validasi.

### 6. Aset Visual
Menggunakan aset SVG lokal berupa logo SisaBaik dan ilustrasi makanan untuk mendukung tampilan antarmuka.

### 7. Penyimpanan Data
- Data latihan berasal dari fixture pada `src/data/penawaran-latihan.js`.
- Perubahan state disimpan sementara di memori halaman Vue.
- Frontend pada Chapter 9 belum melakukan pemanggilan API dan belum menggunakan PostgreSQL untuk menyimpan data.
- Data penawaran yang ditambahkan akan kembali ke data awal setelah halaman di-refresh.

## Capaian Chapter 10

### 1. Integrasi Frontend dan Backend
- Mengintegrasikan frontend Vue 3 dengan backend Express.
- Menghubungkan aplikasi dengan API melalui konfigurasi proxy Vite.
- Menggunakan PostgreSQL sebagai basis data aplikasi.
- Menambahkan helper untuk komunikasi dengan API.

### 2. Autentikasi dan Pengelolaan Akun
- Menambahkan fitur pendaftaran dan login pengguna.
- Mendukung akun pembeli dan penyedia.
- Mengelola sesi autentikasi pengguna.
- Menambahkan halaman profil dan informasi usaha.
- Menerapkan pembatasan akses berdasarkan peran dan status verifikasi.

### 3. Katalog dan Pengelolaan Penawaran
- Menampilkan katalog penawaran dari API.
- Menambahkan pencarian, filter, dan pagination.
- Menampilkan halaman penyedia.
- Mengelola penawaran milik penyedia.
- Mendukung pengelolaan foto dan versi data penawaran.

### 4. Pemesanan dan Pengelolaan Transaksi
- Menambahkan fitur pemesanan makanan.
- Menampilkan daftar dan detail pesanan.
- Mengelola tindakan serta perubahan status pesanan.
- Menerapkan mekanisme reservasi dan pemulihan stok.
- Mendukung pembayaran di tempat, termasuk pesanan gratis.

### 5. Dashboard dan Halaman Informasi
- Menambahkan dashboard untuk menampilkan ringkasan aktivitas.
- Menampilkan metrik terkait belanja, pendapatan, dan penghematan.
- Menambahkan halaman informasi aplikasi.

### 6. Routing dan Tampilan Aplikasi
- Menggunakan Vue Router untuk navigasi ant halaman.
- Menambahkan kerangka aplikasi melalui `App.vue`.
- Menggunakan aset visual dan font lokal.
- Menambahkan halaman registrasi, login, katalog, penyedia, pesanan, dashboard, dan informasi.

## Struktur Berkas Frontend

Direktori frontend berada di dalam folder `frontend/`.

- `index.html`: Dokumen HTML utama.
- `src/main.js`: Entry point aplikasi Vue.
- `src/App.vue`: Komponen utama aplikasi.
- `src/style.css`: Styling dan desain antarmuka.
- `src/assets/`: Logo, ilustrasi SVG, dan aset visual.
- `src/assets/fonts/`: Font lokal.
- `src/components/`: Komponen antarmuka yang dapat digunakan kembali.
- `src/data/`: Data fixture latihan.
- `src/lib/`: Helper komunikasi API, tampilan, dan validasi.
- `src/router/`: Konfigurasi routing aplikasi.
- `src/views/`: Halaman-halaman aplikasi.

## Prasyarat

- Node.js sesuai baseline modul Chapter 10.
- npm.
- PostgreSQL.
- Backend dan frontend SisaBaik.

## Instalasi dan Menjalankan Aplikasi

Pasang dependency backend dari direktori utama proyek:

```bash
npm install
```

Jalankan backend dalam mode development dari direktori utama:

```bash
npm run dev
```

Buka terminal baru, lalu masuk ke direktori frontend:

```bash
cd frontend
```

Pasang dependency frontend:

```bash
npm install
```

Jalankan frontend dalam mode development:

```bash
npm run dev
```

Frontend development server berjalan pada:

`http://127.0.0.1:5173/`

## Build dan Preview Frontend

Dari direktori `frontend/`, jalankan:

```bash
npm run build
```

Untuk melihat hasil build melalui preview:

```bash
npm run preview
```

Preview frontend berjalan pada:

`http://127.0.0.1:4173/`

## Build dan Menjalankan Aplikasi Lokal

Dari direktori utama proyek, jalankan:

```bash
npm run build
```

Kemudian jalankan:

```bash
npm run start:local
```

Untuk kembali ke mode pengembangan, hentikan proses lokal dengan `Ctrl + C`, kemudian jalankan kembali backend dan frontend dalam mode development.

## Konfigurasi Integrasi

Frontend menggunakan proxy `/api` untuk meneruskan permintaan API ke backend Express. Konfigurasi ini membantu komunikasi frontend dan backend pada lingkungan lokal.

Pengelolaan transaksi dilakukan melalui backend agar aturan kepemilikan, status pesanan, reservasi stok, dan perubahan data tetap diperiksa oleh server.

## Hasil Build dan Pemeriksaan

Build frontend dilakukan menggunakan perintah `npm run build`. Hasil kompilasi disimpan pada direktori `frontend/dist`.

Preview hasil build dijalankan menggunakan `npm run preview`.

Pemeriksaan yang dilakukan pada tahap ini terbatas pada menjalankan build dan membuka tampilan aplikasi melalui browser. Pengujian menyeluruh terhadap seluruh fitur dan alur transaksi belum dilakukan.

## Batasan Pengembangan

Beberapa batasan yang masih terdapat pada implementasi Chapter 10 berdasarkan modul:
- Sesi pengguna hilang setelah halaman di-refresh karena token hanya disimpan di memori.
- Belum tersedia payment gateway.
- Belum tersedia fitur reset sandi dan verifikasi email.
- Belum menggunakan idempotency key.
- Belum tersedia pembaruan data secara realtime.
- Penghematan biaya penjual belum dapat dihitung secara aktual.

## Catatan Pengembangan

Chapter 9 menjadi dasar pembangunan antarmuka frontend menggunakan Vue 3. Pada Chapter 10, aplikasi dikembangkan lebih lanjut melalui integrasi frontend, backend, dan PostgreSQL.

Pengembangan berikutnya dapat difokuskan pada penyempurnaan fitur, pengujian integrasi, keamanan, serta penerapan aplikasi pada lingkungan hosting.
