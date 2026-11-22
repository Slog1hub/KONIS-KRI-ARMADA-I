# KONIS KRI — Kalkulator Persentase Kondisi Teknis KRI

Aplikasi web offline (PWA) untuk menghitung Konis KRI (Platform dan Sewaco/Komunikasi). Rumus mengikuti Surat Edaran Aslog Kasal tentang Tata Cara Pemberian Nilai Persentase Kondisi Teknis KRI (Juni 2026) beserta lampirannya.

- Berjalan **offline** setelah dibuka satu kali.
- **Gratis**, tanpa akun, tanpa server, tanpa Play Store.
- Data **hanya tersimpan di HP pengguna**. Aplikasi tidak membuat koneksi internet apa pun karena dikunci Content-Security-Policy. Jika PIN diaktifkan, data terenkripsi AES-256.

## Isi folder

| File | Fungsi |
|---|---|
| `index.html` | Seluruh aplikasi (tampilan, rumus, penyimpanan) |
| `sw.js` | Service worker: menyimpan aplikasi agar bisa dibuka tanpa internet |
| `manifest.webmanifest` | Agar bisa dipasang ke layar utama HP |
| `icons/` | Ikon aplikasi |

---

## Langkah instalasi

Aplikasi perlu ditaruh **satu kali** di alamat `https://`. Setelah itu setiap perwira cukup membuka tautannya di HP dan memasangnya. Pilih salah satu cara berikut.

### Cara A — GitHub Pages (gratis, disarankan)

1. Buat akun di github.com (gratis).
2. Klik **New repository**. Beri nama, misalnya `konis-kri`, pilih **Public**, lalu klik **Create repository**.
3. Klik **uploading an existing file**. Seret **isi** folder ini (`index.html`, `sw.js`, `manifest.webmanifest`, dan folder `icons`), lalu klik **Commit changes**.
4. Buka **Settings → Pages**. Pada *Branch*, pilih `main` dan folder `/ (root)`, lalu klik **Save**.
5. Tunggu 1–2 menit. Alamat aplikasi muncul, misalnya `https://namaakun.github.io/konis-kri/`.

> Yang menjadi publik hanya kode aplikasi. Data kapal yang diisi perwira **tidak pernah** dikirim ke GitHub karena tersimpan di HP masing-masing.

### Cara B — Netlify Drop (gratis, tanpa Git)

1. Buka app.netlify.com/drop.
2. Seret folder `konis-kri` ke halaman itu.
3. Salin alamat `https://….netlify.app` yang muncul. Buat akun gratis agar situsnya tidak terhapus.

### Cara C — Server intranet satuan

Salin isi folder ke server web internal yang memakai **HTTPS**. Tidak perlu database maupun bahasa server.

### Memasang di HP (untuk setiap perwira)

**Android (Chrome)**
1. Buka alamat aplikasi di Chrome.
2. Ketuk menu ⋮, lalu pilih **Instal aplikasi** atau **Tambahkan ke layar utama**.
3. Ikon **KONIS KRI** muncul di layar utama. Mulai sekarang aplikasi bisa dibuka tanpa sinyal.

**iPhone (Safari)**
1. Buka alamat aplikasi di Safari.
2. Ketuk **Bagikan** (kotak dengan panah ke atas), lalu pilih **Tambahkan ke Layar Utama**.

### Mencoba di laptop tanpa hosting

- Klik dua kali `index.html`. Aplikasi berjalan, tetapi tidak bisa dipasang sebagai aplikasi.
- Bila Python terpasang, jalankan `python -m http.server 8000` di dalam folder, lalu buka `http://localhost:8000`.

### Memperbarui aplikasi

Ganti file di hosting, lalu naikkan versi di baris `const CACHE = 'konis-kri-v1.0.0';` pada `sw.js` (misalnya menjadi `v1.0.1`). HP pengguna mengambil versi baru saat aplikasi dibuka sambil online. Data pengguna tidak hilang.

---

## Cara pakai singkat

1. **+ Tambah Kapal**: isi nama KRI, lalu pilih **Kapal Tempur** (bobot 40:60) atau **Kapal Bantu** (bobot 60:40). Bobot bisa diubah, misalnya 50:50 sesuai rumus C SE.
2. Centang pesawat yang terpasang dan atur jumlah unitnya (misalnya 4 MPK, 3 DG).
3. Buka tiap pesawat atau unit, lalu isi **nilai standar** dan **nilai saat ini**. Persentase dan rumusnya tampil langsung.
4. Konis Total dan statusnya (B/RR/RB) diperbarui otomatis. Bila ada komponen vital di bawah 60%, muncul peringatan merah.
5. Buka **Laporan**: isi penandatangan, putuskan penetapan RB bila perlu, lalu **Cetak/PDF**, **CSV** (Excel), atau **Simpan** ke riwayat periode.
6. **Pengaturan**: aktifkan PIN, ekspor atau impor cadangan untuk pindah HP atau berbagi ke perwira lain.

## Aturan perhitungan

| Hal | Aturan |
|---|---|
| Parameter "makin besar makin baik" (RPM, daya, tekanan, tebal plat, jumlah siap) | saat ini ÷ standar × 100% |
| Parameter "makin kecil makin baik" (ampere, waktu, temperatur bantalan) | standar ÷ saat ini × 100%; ≤ standar = 100% |
| Suhu FR/AC | tercapai ≤ seharusnya = 100%; selain itu perbandingan |
| Keakuratan | (1 − deviasi ÷ referensi) × 100% |
| Life Raft | (1 − bulan lewat ÷ 12) × 100%; sisa hari dibulatkan menjadi 1 bulan |
| Alkom | Σ bobot × (siap ÷ standardisasi); alat yang standarnya 0 tidak dihitung |
| Display, panel, dll. | skala 100/75/50/25/0 persis seperti tabel SE |
| Nilai unit | rata-rata parameternya |
| Nilai pesawat | rata-rata unitnya |
| Konis Platform / Sewaco | rata-rata nilai pesawat di kelompok itu |
| Konis Total | (bobot P × Platform) + (bobot S × Sewaco/Kom) |
| Status | ≥ 86% Baik/Siap Tempur; 60–85,99% Rusak Ringan/Siap Operasi; < 60% Rusak Berat/Tidak Siap |

Setiap parameter dibatasi 0–100%. Bila contoh di SE menulis pembagi yang tidak sama dengan jumlah baris (mis. Sonar "∑6" untuk 7 parameter), aplikasi membagi dengan jumlah parameter yang benar-benar dinilai.
