# FMV Studio — build 0.3

Tujuan proyek: membuat save editor untuk file `.fmv` Football Manager Touch, yang bisa dijalankan dari browser Android dan di-host di GitHub Pages.

Format yang terdeteksi dari isi file sampel adalah FM21 Touch `21.4.0` (`21.4.0-0vmf.` dan `21.4.0-0tmf.`), meskipun nama salah satu file mengandung teks “FMT 26”. Rincian hasil verifikasi ada di [FORMAT-RESEARCH.md](FORMAT-RESEARCH.md).

## Apa yang benar-benar bekerja pada build ini
- Membuka **satu** file FMV.
- Memvalidasi header `02 01 fmf.` dan menemukan manifest melalui pointer di header.
- Mendekompresi manifest Zstandard menggunakan `fzstd` dari CDN.
- Membaca daftar anggota arsip dan sub-arsip (termasuk `rgman`).
- Mencari dan memfilter anggota arsip.
- Mengekstrak anggota arsip yang dipilih sebagai file biner.
- Mengunduh indeks manifest sebagai JSON.
- Semua file save diproses di browser; file save tidak dikirim ke server.

## Yang belum selesai
Build ini **belum** mengedit data pemain, klub, liga, kontrak atau transfer dan belum menulis ulang file `.fmv`. Itu membutuhkan parser database khusus untuk versi internal save yang dibaca dan encoder/repacker yang tervalidasi. Jangan gunakan hasil ekstraksi sebagai pengganti save game.

## Menjalankan di GitHub Pages
1. Buat repository publik bernama `fmv-studio`.
2. Unggah `index.html` ke root repository.
3. Buka Settings → Pages → Deploy from a branch → `main` → `/(root)`.
4. Buka URL GitHub Pages dari Android. Browser perlu akses internet untuk memuat modul `fzstd` dari esm.sh.

## Catatan format
Manifest memiliki nama save, daftar member dengan offset relatif ke payload, ukuran tersimpan, ukuran hasil dekompresi, serta sub-arsip. Build ini memvalidasi rentang dan ukuran hasil dekompresi sebelum mengunduh anggota. Offset/struktur field pemain tidak ditebak.
