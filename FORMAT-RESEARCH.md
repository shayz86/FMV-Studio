# FMV Studio 0.4 — Format notes and validation

Target: Football Manager Touch 2021, internal save versions 21.3.x–21.4.x.

## Verified container facts

- Save container signature: `02 01 66 6d 66 2e` (`fmf.`).
- The manifest container offset is `9 + u32le(file, 9)`.
- The manifest Zstandard frame starts 13 bytes after that offset.
- Member payload frames begin at byte 30 plus the member's relative offset.
- The manifest contains each member's relative offset, stored length, plain length and 16-byte stamps; `rgman` is represented as a child archive.
- Each member can comprise multiple Zstandard frames. The frames are 131,072 uncompressed bytes in the observed main database chunks except for the final chunk. A 4-byte little-endian next-frame length is stored between frames. Each member's stored length also includes the 4-byte boundary marker after its final frame.
- The final member boundary overlaps the first four bytes of the manifest wrapper. The writer preserves this layout and adjusts the top-level pointer when member sizes change.
- `game_db.dat` begins with `03 01 74 61 64 2e` (`tad.`) and contains the internal database version marker (`2130` or `2140` in the provided samples).

## Round-trip test performed during development

A local repacking test changed one same-length database string (`Mohamed Salah` → `Mohamed Salih`) in a copy of the 21.4 sample, re-compressed the database in its original chunk sizes, rebuilt the manifest and offsets, reopened the generated archive, and verified that the decompressed database matched the edited database byte-for-byte. The generated test file was not validated inside the game, so this proves container-level round-trip only, not game acceptance.

## Text edit constraints

- Search returns byte offsets and surrounding context so repeated text occurrences are distinguishable.
- The edit is allowed only when a 32-bit byte-length value immediately before the string matches its UTF-8 byte length.
- The replacement must preserve UTF-8 byte length and cannot contain NUL. This preserves the database size and all internal byte offsets.
- A string pool may be shared by more than one entity. Until entity records are mapped, a text occurrence cannot be promised to belong exclusively to one player or club.

## Not yet reverse-engineered

Player/person record layout, player attributes and CA/PA, club and competition record schemas, contract linkage, transfer relationship records, and safe length-changing record edits. These must be independently validated for 21.3 and 21.4; FM26 layouts are not assumed to apply.
