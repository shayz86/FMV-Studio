# Roadmap

1. **Done in 0.4:** archive reader, database extraction, structured text search/edit with fixed byte length, Zstandard chunk recompression, manifest/offset writer, container round-trip validation.
2. **Next:** identify and validate FM21.3/21.4 person/player record boundaries and map player records to name IDs.
3. Build player grid/editor: name, date of birth, nationality, positions, attributes, current/potential ability and other verified fields.
4. Identify club and competition records and build searchable club/league editors.
5. Map contract, current club/team and transfer-related relationships; implement transfer operations with consistency checks.
6. Add length-changing edits only after a record serializer is verified; support rollback and an edit journal.
7. Validate generated saves in FMT21 Touch on copies of real saves.
