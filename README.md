# FMV Studio — Prototype 0.1

Aplikasi web statis untuk menganalisis file `.fmv` secara lokal di browser Android. Dapat di-host di GitHub Pages atau Cloudflare Pages.

## Fitur prototipe
- Pilih dua file untuk perbandingan.
- Analisis ukuran, header awal, entropi sampel, dan signature kandidat.
- Cari string ASCII yang dapat dibaca pada sampel awal.
- Hex viewer untuk membaca byte berdasarkan offset.
- Ekspor laporan analisis JSON.
- Semua file diproses lokal pada browser; file tidak diunggah ke server.

## Batasan penting
Prototype ini **belum** melakukan ekstraksi file/folder, editing tabel payload, atau repack `.fmv`. Format internal belum diverifikasi. Penanda byte yang mirip signature kompresi tidak cukup untuk membuktikan bahwa keseluruhan payload dapat didekompresi.

## Deploy ke GitHub Pages dari Android
1. Buat repository baru, misalnya `fmv-studio`.
2. Unggah `index.html`, `manifest.webmanifest`, dan `sw.js` ke root repository.
3. Buka **Settings → Pages**.
4. Pada Build and deployment, pilih **Deploy from a branch**, branch `main`, folder `/ (root)`, lalu Save.
5. Buka URL Pages yang muncul.

Untuk Cloudflare Pages, buat project Pages yang terhubung ke repository dan gunakan root directory `/` tanpa build command.

## Tahap selanjutnya
Validasi struktur wrapper dengan analisis byte yang lebih lengkap dan beberapa file pembanding. Setelah parser dapat memvalidasi dan merekonstruksi payload, baru tambahkan ekstraksi serta editor berdasarkan tipe resource yang benar-benar ditemukan.
