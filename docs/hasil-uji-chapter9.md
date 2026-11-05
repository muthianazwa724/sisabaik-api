# Laporan Hasil Pengujian – Chapter 9 Antarmuka Vue SisaBaik

- **Tanggal Pengujian**: November 2026
- **Penguji**: Muthia Nazwa Marlina
- **NIM**: 23367036
- **Lingkungan**: Node.js v26.10.0, Vue 3, Vite
- **Commit**: Diisi setelah proses commit

---

## Matriks Hasil Pengujian Aktual

| No. | Skenario Pengujian | Ekspektasi | Hasil Aktual | Status |
|---|---|---|---|---|
| 1 | Membuka katalog awal | Katalog tampil dengan data penawaran | 3 kartu penawaran, total 15 unit, dan 2 penyedia tampil. | Berhasil |
| 2 | Pencarian kata LAPAU | Hanya penawaran yang sesuai ditampilkan | Tampil 2 penawaran yang sesuai. | Berhasil |
| 3 | Pencarian produk tidak tersedia | Empty state ditampilkan | Daftar penawaran kosong dan empty state muncul. | Berhasil |
| 4 | Filter kategori Roti | Penawaran kategori Roti ditampilkan | Tampil 1 penawaran, yaitu Paket Roti Sore. | Berhasil |
| 5 | Memilih penawaran | Pesan konfirmasi muncul | Muncul pesan "Pilihan Anda diperbarui." dan stok tetap 6. | Berhasil |
| 6 | Beralih ke mode Penyedia | Ringkasan penawaran milik penyedia ditampilkan | Tampil 3 penawaran Rina, 2 tersedia, dan total 9 unit. | Berhasil |
| 7 | Mengirim formulir kosong | Validasi menolak input kosong | Pesan kesalahan validasi muncul dan daftar tetap 3 kartu. | Berhasil |
| 8 | Harga penawaran lebih tinggi dari harga normal | Validasi menolak harga tidak sesuai | Pesan kesalahan validasi muncul dan daftar tetap 3 kartu. | Berhasil |
| 9 | Mengirim formulir penawaran valid | Penawaran baru ditambahkan | Formulir valid telah diisi dan dikirim. Hasil jumlah kartu setelah pengiriman belum tercatat. | Perlu verifikasi |
| 10 | Refresh browser | Data fixture kembali ke kondisi awal | Katalog kembali menampilkan 3 kartu, 15 unit, dan 2 penyedia. | Berhasil |
| 11 | Memeriksa logo, ilustrasi, dan badge | Aset visual tampil dengan benar | Logo, ilustrasi, dan badge tampil pada halaman. | Berhasil |
| 12 | Menjalankan production build | Proses build selesai tanpa error | 23 modul berhasil diproses dan folder `dist` terbentuk. | Berhasil |
| 13 | Menjalankan preview | Aplikasi dapat dibuka dari hasil build | Halaman berhasil dibuka melalui alamat lokal preview. | Berhasil |

---

## Aliran Komunikasi Props dan Emits

Komunikasi antar-komponen pada aplikasi SisaBaik menggunakan mekanisme props dan emits.

1. **App.vue dan FilterPenawaran.vue**
   - App.vue mengirimkan data pencarian, kategori, dan daftar kategori melalui props.
   - FilterPenawaran.vue mengirimkan perubahan pencarian, kategori, dan aksi reset melalui emits.

2. **App.vue dan KartuPenawaran.vue**
   - App.vue mengirimkan data penawaran dan informasi yang diperlukan kartu melalui props.
   - KartuPenawaran.vue mengirimkan aksi pemilihan penawaran melalui emits.

3. **App.vue dan FormPenawaran.vue**
   - App.vue memberikan data yang diperlukan formulir melalui props.
   - FormPenawaran.vue mengirimkan data penawaran baru melalui emits setelah validasi berhasil.

---

## Perhitungan Ringkasan

### 1. Mode Pembeli

Berdasarkan data fixture awal:

- Jumlah penawaran: 3 kartu.
- Total unit tersedia: 15 unit.
- Jumlah penyedia: 2 penyedia.

### 2. Mode Penyedia

Berdasarkan data penawaran milik Rina:

- Jumlah seluruh penawaran: 3 penawaran.
- Jumlah penawaran tersedia: 2 penawaran.
- Total unit tersedia: 9 unit.

---

## Hasil Build dan Preview

Perintah production build:

```bash
npm run build
```

Hasil aktual:
- Build berhasil tanpa error.
- Sebanyak 23 modul berhasil diproses.
- Folder `dist` berhasil dibuat.

Perintah preview:

```bash
npm run preview
```

Hasil aktual:
- Aplikasi berhasil dijalankan melalui server preview lokal.
- Halaman katalog dapat dibuka.
- Filter penawaran dapat digunakan.
- Mode penyedia dan formulir dapat dibuka.

---

## Dokumentasi Screenshot

Screenshot yang perlu dilampirkan sebagai bukti pengujian:

1. Tampilan katalog awal.
2. Tampilan empty state.
3. Formulir dengan validasi tidak valid.
4. Formulir dengan data valid.
5. Tampilan aplikasi pada perangkat mobile.
6. Terminal yang menunjukkan hasil production build.

---

## Catatan Pengujian

1. Aplikasi Chapter 9 menggunakan data fixture di memori.
2. Penambahan data tidak disimpan secara permanen. Setelah browser di-refresh, data kembali ke kondisi awal.
3. Browser belum melakukan pemanggilan API pada modul ini.
4. Backend Modul 8 tetap memiliki fitur pendaftaran mandiri, verifikasi penyedia, autentikasi, dan otorisasi.
5. Integrasi frontend dengan backend direncanakan pada pertemuan 11.
6. File `.env` backend, kata sandi, hash, JWT, dan data akun nyata tidak disertakan dalam dokumentasi.

