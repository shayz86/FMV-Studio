# FMV Studio 0.4 — FMT21 Touch Save Editor Core

FMV Studio targets **Football Manager Touch 2021**, internal save versions **21.3.x and 21.4.x**. The filename is not used to infer the game version; the internal `game_info.dat` version is read instead.

## Working functionality in 0.4

- Opens one `.fmv` save in the browser.
- Reads the FMF/FMV archive manifest and identifies `game_db.dat`.
- Decompresses the database while preserving original Zstandard chunk boundaries.
- Searches for a text value in the decompressed database and shows each occurrence with a byte offset and surrounding context.
- Allows editing an occurrence only when a 32-bit byte-length prefix is detected and the replacement has **exactly the same UTF-8 byte length**. This avoids shifting any binary offsets.
- Re-compresses the modified database chunk-by-chunk, updates archive offsets and stored lengths, serializes/compresses the manifest, and builds a new `.fmv`.
- Re-opens the generated manifest and compares every decompressed `game_db.dat` byte against the edited database before enabling the download.
- Processes save data locally in the browser; it is not uploaded to a server.

## Important limitation

This build is a **real, limited text-field editing/repacking core**, not yet the full entity editor. It does **not** yet map a selected string to a verified player, club or competition record. It does not edit numeric player attributes, CA/PA, positions, club finances, league rules, contracts, or transfer relationships. Do not treat an occurrence in the text search as a guaranteed player-specific field; a name-pool value can be shared. Those require a verified FM21.3/21.4 record parser and relationship mapper.

The app intentionally refuses length-changing edits. It does not create or delete database records.

## Deploy to GitHub Pages from Android

1. Download and extract `fmv-studio-v0.4.zip`.
2. Create a GitHub repository named `fmv-studio`.
3. Upload `index.html` and the documentation files to the repository root.
4. In **Settings → Pages**, choose **Deploy from a branch**, branch `main`, folder `/(root)`, then save.
5. Open the Pages URL in Chrome on Android.

The app uses two browser modules from CDNs: `fzstd` for decompression and `@bokuweb/zstd-wasm` for compression. Internet access is required when first loading the app.

## Safety

- Always keep an untouched copy of the original save.
- The built-in check proves that the output archive can be parsed and its database round-trips byte-for-byte. It does **not** prove that the game accepts every edited field. Test on a duplicate save.
- The project is unofficial and is not affiliated with Sports Interactive or SEGA.
