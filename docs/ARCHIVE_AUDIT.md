# Uploaded Archive Audit

## High-level comparison

| Archive | Notebook count | Audit conclusion |
|---|---:|---|
| `Notebook.zip` | 20 | Chosen as canonical source. Contains the complete set, including the hyperparameter-sensitivity notebook. |
| `Colab Notebooks.zip` | 19 | Primarily duplicate Colab copies, often with execution outputs stored. |

Across the 19 same-named notebook pairs:

- many are byte-identical;
- almost all remaining pairs have identical code cells and differ only in stored outputs and/or trivial markdown formatting;
- the intermediate cubic mitigation split-run has extra diagnostic code in the Colab copy;
- no Colab-only scientifically distinct final notebook was identified.

## Why `Notebook.zip` is canonical

1. It has the larger and more complete notebook set (20 vs. 19).
2. It includes the hyperparameter-sensitivity notebook absent from the Colab archive.
3. For the final notebooks retained in this repository, using a single archive avoids mixed provenance.
4. Smaller source-oriented copies are preferable for GitHub where the code is identical to output-heavy Colab copies.

## Integrity policy

The packaging process performs byte-for-byte copies only. After copying, SHA-256 hashes of each source and destination notebook are compared. The final hashes are recorded in `SOURCE_MANIFEST.csv` and `SHA256SUMS.txt`.
