# Analisis awal dua file FMV (2026-10-28)

Analisis dilakukan secara read-only pada dua file yang diberikan. File asli tidak diubah.

| Properti | `FMT 26 BY WAHYU.fmv` | `2025-SEPTEMBER ( 100% ).fmv` |
|---|---:|---:|
| Ukuran | 41,450,093 byte | 53,584,441 byte |
| Awalan byte | `02 01 66 6d 66 2e 08 00 00 72 50 78 02 00 00 00 00 11 00 00 00 00 00 00 00 03 8b 01 00 00 28 b5 2f fd` | `02 01 66 6d 66 2e 08 00 00 35 78 31 03 00 00 00 00 11 00 00 00 00 00 00 00 03 8b 01 00 00 28 b5 2f fd` |
| Signature `28 b5 2f fd` | offset 30 | offset 30 |
| Entropi sampel awal 64 KiB | sekitar 7.9885 bit/byte | sekitar 7.9817 bit/byte |

## Temuan dan batas keyakinan

- Kedua file memiliki 30 byte awal yang mirip; ada perbedaan pada beberapa byte awal yang kemungkinan menyimpan metadata atau identitas file, tetapi maknanya belum dipastikan.
- Empat byte `28 b5 2f fd` cocok dengan signature standar Zstandard. Namun uji dekompresi standar pada byte mulai offset 30 gagal dengan pesan `unsupported format`. Jadi belum sah menyebut payload ini sebagai frame Zstandard valid. Bisa jadi signature tersebut kebetulan muncul, header/stream memakai framing khusus, atau data memiliki lapisan transformasi lain.
- Entropi tinggi menunjukkan data sangat biner/acak pada sampel, tetapi tidak membuktikan apakah data dikompresi atau dienkripsi.
- Belum ada struktur file/folder internal yang berhasil divalidasi. Karena itu tidak dilakukan ekstraksi, edit payload, atau repack.

## Langkah analisis berikutnya

1. Cari dokumentasi atau implementasi format yang tepat untuk ekstensi `.fmv` dan versi game pembuatnya.
2. Bandingkan beberapa save yang dibuat oleh game yang sama dengan perubahan tunggal yang diketahui (misalnya satu perubahan nama klub/pemain), sambil mempertahankan backup.
3. Petakan header dan kemungkinan checksum/ukuran/indeks menggunakan bukti berulang.
4. Hanya aktifkan ekstraksi setelah parser bisa memvalidasi ukuran, batas blok, dan hasil rekonstruksi.
5. Uji repack hanya pada salinan dan verifikasi file dapat dimuat game sebelum menyatakan kompatibel.
