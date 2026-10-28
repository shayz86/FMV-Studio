# Verified FMV format observations

These observations were checked against the two sample `.fmv` files provided for this project. The save files themselves are not included in this repository.

## Detected versions

| Sample | `game_info.dat` version | Main member count | `rgman` child count | `game_db.dat` stored | `game_db.dat` decompressed |
|---|---|---:|---:|---:|---:|
| Sample A | `21.4.0-0vmf.` | 71 | 764 | 30,243,327 B | 108,619,254 B |
| Sample B | `21.4.0-0tmf.` | 73 | 765 | 32,139,312 B | 112,882,393 B |

The file names alone are not treated as evidence of game version. The internal version string was read from `game_info.dat`.

## Container facts verified

- Header starts with bytes `02 01 66 6d 66 2e` (`02 01 fmf.`).
- In these samples, the manifest container is found at `9 + u32le(file, 9)`.
- The manifest's Zstandard frame begins 13 bytes after the manifest-container start.
- The main compressed payload begins at byte 30.
- The manifest names members and gives each member's relative offset, stored length, and uncompressed length.
- The manifest has a top-level member list and a `rgman` sub-archive containing competition/rule-group data.
- Main members can span many Zstandard frames. The stream stores a 4-byte little-endian next-frame size between frames. Searching for Zstandard magic alone is not a safe frame index.
- The `game_db.dat` member was successfully decompressed and its resulting byte count matched the manifest's `plain` length in Sample A. Its prefix is `03 01 74 61 64 2e` (`03 01 tad.`).

## Important limits

These are container-level findings. They do **not** yet establish player, club, league, contract, or transfer field offsets for FM21.4.0. FM26/FM24 field layouts must not be copied into this version without independent validation. The current web app therefore extracts members but does not claim to edit or re-pack a save.
