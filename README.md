# FMV Editor — MVP 0.1

Browser-based FMV file explorer for FM Touch 21 sample files.

## Run
Open `index.html` in a modern browser with internet access (fzstd is loaded from jsDelivr). For GitHub Pages, upload `index.html` to the repository root and enable Pages.

## Current capabilities
- Load `.fmv` locally in the browser
- Read file header and file size
- Scan Zstandard magic markers
- Attempt decompression of the first 16 frames for inspection
- Search readable ASCII fragments
- Hex viewer
- Save an unchanged backup copy

## Important
This is an initial format-inspection build, not yet a full editor. It deliberately does not rewrite FMV records. Record boundaries, field types, compression framing, checksums, and safe rebuild rules still need to be reverse-engineered and validated against the supplied sample before enabling edits.
