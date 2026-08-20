# File Selection and Deduplication

## Selection rule

The GitHub package keeps one authoritative notebook for each scientifically distinct final experiment. Source cells were not edited.

### Included

All 14 included notebooks are listed in `NOTEBOOK_INDEX.md` and `SOURCE_MANIFEST.csv`.

### Excluded from `Notebook.zip`

| File | Decision | Reason |
|---|---|---|
| `RC_HAVOK_AB_Rounding_Ablation_SMOKETEST.ipynb` | Excluded | Smoke-test predecessor; superseded by RC_HAVOK_AB_Rounding_Ablation_FINAL.ipynb. |
| `RC_HAVOK_Confirmation_Run.ipynb` | Excluded | Compact/result-summary confirmation notebook; superseded for code release by the FULL reproducible confirmation notebook. |
| `RC_HAVOK_Noise_Mitigation_Cubic_Run.ipynb` | Excluded | Intermediate split-run notebook; final combined Fixed notebook covers PWL and cubic mitigation in one reproducible workflow. |
| `RC_HAVOK_Noise_Mitigation_PWL_Run.ipynb` | Excluded | Intermediate split-run notebook; final combined Fixed notebook covers PWL and cubic mitigation in one reproducible workflow. |
| `RC_HAVOK_Noise_Mitigation_Regularized.ipynb` | Excluded | Earlier regularized mitigation version; superseded by RC_HAVOK_Noise_Mitigation_Regularized_Fixed.ipynb. |
| `RC_HAVOK_Noise_Robustness.ipynb` | Excluded | Earlier noise-robustness version; superseded by the V2_Q1 statistical revision. |

## Cross-archive duplicates

`Colab Notebooks.zip` was used as a cross-check rather than as a second source tree. Most same-named notebooks have identical code cells; differences are predominantly stored execution outputs. Keeping both archives would create duplicate source files and make it unclear which copy is canonical.

The one meaningful code-cell difference observed across same-named archive copies was in the intermediate `RC_HAVOK_Noise_Mitigation_Cubic_Run.ipynb`, where the Colab copy contained extra display/diagnostic code (`@title` metadata text and a current-directory listing). That intermediate notebook is excluded anyway because the final combined Fixed mitigation notebook supersedes it.
