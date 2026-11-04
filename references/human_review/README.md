# Human-review reference sheets

These are historical review records supplied for reference. They are not training inputs, not a new review, and not an automatic approval of the current execution.

## Files

- [human_review_primary.csv](human_review_primary.csv): original unfilled review sheet, with reviewer fields blank.
- [human_review_primary_filled.csv](human_review_primary_filled.csv): completed submitted sheet, with the reviewer recorded as Ayman for all 66 records.

Both files are copied byte for byte from the earlier preparation/review records. They are CSV files readable in Excel; no XLSX conversion or alteration has been performed. In Excel, use Data > From Text/CSV with UTF-8 encoding, and import identifier columns as text.

## Coverage and interpretation

The review covers 66 sampled examples across 22 primary structural templates (three examples per template), not all 3,759 questions. The sheets contain original prompts, adapted questions, canonical answers, bounds and review fields. They document checks of answer correctness, wording clarity, units/rounding and adaptations. They were retained in the earlier replication preparation stage. A review date is not recorded in these sheets; no date has been inferred from filesystem timestamps.

The recorded validation found matching example membership, no unresolved approval issues, and no substantive changes to the base data. It reported 138 formatting-only numeric cells across 66 rows (for example, 71.9550 versus 71.955). The submitted completed file is preserved as submitted, without rewriting those cells. Consequently, the before/after files need not be byte-identical in their numerical text formatting.

## Link to the executable dataset

Use example_id to match rows to ../../split_membership.csv and to the processed records generated under the experiment output's 01_data directory. All 66 reference IDs are present in the packaged membership specification. The historical sheets themselves are not the full dataset.

This execution reconstructs the previously curated specification from fresh downloads and verifies all five processed partition checksums against ../../reference_allocation_hashes.json. It inherits the earlier sampled review rather than performing a new review. Matching checksums establish unchanged files, not independent semantic correctness. Changes to question wording, calculation rules or reference answers require renewed review of affected records.

## File integrity

- `human_review_primary.csv`: SHA-256 `1b1c933a3f8a654a318b9ca0e13bb7c3d35f3f8b368bc725aa59dc6d31f3d0cc`
- `human_review_primary_filled.csv`: SHA-256 `3eef361000a83fbb49f8a23f9d68bf8424cdcc91b4a26b477af18ba5b15e9674`

The sheets are supplemental documentation. Do not edit them to resolve a training or runtime error. They are not loaded to grant approval by the experiment workflow. The existing package-identity mechanism inventories CSV files, including these references; therefore do not add this reference folder midway through an already-started run under the same output directory. Use the complete rebuilt package for a fresh run, or retain the unchanged prior package for an existing run.
