# SisaBaik – Chapter 9

Developed by: Muthia Nazwa Marlina  
NIM:23367036

SisaBaik merupakan proyek mata kuliah Pengembangan Web yang dikembangkan untuk membantu penyedia memulihkan nilai stok makanan berlebih melalui penjualan dengan harga khusus atau penyaluran. Aplikasi ini juga membantu masyarakat memperoleh makanan yang lebih terjangkau dan mengurangi limbah pangan.

Pada Chapter 9, pengembangan difokuskan pada pembangunan antarmuka frontend menggunakan Vue 3 dan Vite.

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
- Frontend pada chapter ini belum melakukan pemanggilan API dan belum menggunakan PostgreSQL untuk menyimpan data.
- Data penawaran yang ditambahkan akan kembali ke data awal setelah halaman di-refresh.

## Struktur Berkas Frontend

Direktori frontend berada di dalam folder `frontend/`.

- `index.html`: Dokumen HTML utama.
- `src/main.js`: Entry point aplikasi Vue.
- `src/App.vue`: Komponen utama aplikasi.
- `src/style.css`: Styling dan desain antarmuka.
- `src/assets/`: Logo dan ilustrasi SVG.
- `src/components/`: Komponen kartu, filter, formulir, dan ikon.
- `src/data/`: Data fixture latihan.
- `src/lib/`: Helper tampilan, visual, dan validasi formulir.

## Prasyarat

- Node.js minimal v22.0.0.
- npm.

## Instalasi dan Menjalankan Frontend

Masuk ke direktori frontend:

```bash
cd frontend
```

Pasang dependency:

```bash
npm install
```

Jalankan development server:

```bash
npm run dev
```

Buat production build:

```bash
npm run build
```

Jalankan preview hasil build:

```bash
npm run preview
```

Development server berjalan pada:

`http://127.0.0.1:5173/`

Preview hasil build berjalan pada:

`http://127.0.0.1:4173/`

## Hasil Build dan Pengujian

Proses build dilakukan menggunakan perintah `npm run build`.

Build yang berhasil menghasilkan direktori `frontend/dist` yang berisi `index.html` dan aset hasil kompilasi.

Hasil build kemudian diperiksa melalui preview browser. Pengujian dilakukan terhadap katalog, pencarian, filter kategori, pergantian mode tampilan, dan formulir penawaran.

## Catatan Pengembangan

Chapter 9 berfokus pada pembangunan antarmuka frontend menggunakan Vue 3. Data yang digunakan masih berupa fixture latihan dan belum terhubung dengan API backend.

Integrasi frontend dengan backend, autentikasi pengguna, dan penyimpanan data melalui PostgreSQL akan dikembangkan pada pertemuan berikutnya.