# Roadmap FMV Studio

1. **Archive/container** — membuka save, membaca manifest, mengekstrak member. Implementasi awal pada build 0.3.
2. **Database reader untuk versi yang terdeteksi** — parse `game_db.dat`, tabel nama, entitas pemain/staf, klub, kompetisi/liga, kontrak, dan hubungan transfer. Setiap field harus memiliki bukti offset/relasi dan validasi.
3. **Editor berbasis entitas** — pencarian pemain/klub/liga, form atribut, edit kontrak, transfer dan relasi klub. Tidak mengizinkan menyimpan field yang belum dipahami.
4. **Writer/repacker** — serialisasi ulang database/member, kompresi Zstandard, pembaruan manifest, pointer, ukuran, dan checksum sesuai format.
5. **Validasi round-trip** — buka ulang hasil ekspor, bandingkan field yang diubah, validasi panjang/offset, dan uji di salinan save dalam game.
6. **Perluasan** — staf, stadion, keuangan, aturan liga, kompetisi, sejarah dan data lain setelah skema teruji.

## Aturan keamanan
- Selalu pertahankan file asli.
- Tidak menebak field berdasarkan kemiripan format versi game lain.
- Jangan aktifkan ekspor save yang diedit sebelum uji round-trip berhasil.
