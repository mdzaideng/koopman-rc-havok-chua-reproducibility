# Koopman–HAVOK / RC-HAVOK Modeling of Chaotic Chua Dynamics

**Reproducibility code companion for the manuscript**  
**_Spectral and Parameter Sensitivity Analysis of Koopman–HAVOK Reduced-Order Models for Chaotic Chua Dynamics_**

[![DOI](https://zenodo.org/badge/1340288797.svg)](https://doi.org/10.5281/zenodo.22022181)

**Archived release:** v1.0.0  
**Release DOI:** https://doi.org/10.5281/zenodo.22022182

**Authors**  
Md. Zaid Hossain — Khulna University of Engineering and Technology (KUET)  
Borhan Ul Arif — Southeast University  
Md. Sarowar Hossain Rana — Southeast University *(corresponding author)*

This repository contains the curated computational notebooks used for the RC-HAVOK / Koopman–HAVOK Chua-circuit study. The code archive covers the published-result reproduction, cubic Chua extension, coefficient-rounding ablation, seed stability, hyperparameter sensitivity, Gaussian input-noise robustness and mitigation, forcing-coefficient diagnostics, finite-time Lyapunov analysis, exact-SVD confirmation, and HAVOK-only / ESN-only diagnostic baselines.

> **Important interpretation note**  
> The HAVOK-based `R²` and RMSE values in this project quantify **forced reduced-coordinate reconstruction**. The observed HAVOK forcing coordinate is supplied during forward integration, and the identification/evaluation data originate from the same simulated trajectory. These values are **not autonomous forecasting or held-out generalization metrics**.

---

## Repository status

- **Code package:** curated, publication-oriented notebook release
- **Included final notebooks:** 14
- **Canonical packaging source:** `Notebook.zip`
- **Notebook source cells:** preserved byte-for-byte during repository packaging
- **Notebook integrity:** SHA-256 hashes provided in `SHA256SUMS.txt` and `docs/SOURCE_MANIFEST.csv`
- **GitHub repository:** `https://github.com/mdzaideng/koopman-rc-havok-chua-reproducibility`
- **Archived release:** `v1.0.0`
- **Release DOI:** `https://doi.org/10.5281/zenodo.22022182`
- **All-versions DOI:** `https://doi.org/10.5281/zenodo.22022181`
- **License:** BSD 3-Clause
The repository intentionally excludes superseded, duplicate, smoke-test, and split-intermediate notebooks. See [`docs/FILE_SELECTION.md`](docs/FILE_SELECTION.md) for the full selection rationale.

---

## Scientific context

The project builds on the RC-HAVOK framework and the Chua-circuit application reported in:

1. G. Yılmaz Bingöl, O. A. Soysal, and E. Günay, **“Model reduction of dynamical systems with a novel data-driven approach: The RC-HAVOK algorithm,”** *Chaos*, 34, 083143 (2024). DOI: `10.1063/5.0207907`.
2. G. Yılmaz Bingöl and E. Günay, **“Data-Driven Modeling of the Koopman-Oriented Chua Circuit Based on Reservoir Computers,”** *2025 IEEE International Symposium on Circuits and Systems (ISCAS)* (2025). DOI: `10.1109/ISCAS56072.2025.11044234`.

The current study does **not** claim that this repository is an original-author code release for the ISCAS paper. The base paper does not expose every implementation choice required for an exact numerical replication. The reproduction notebook therefore documents the inferred and corrected choices required to obtain the reported bounded Chua dynamics and a close Table-II-style reproduction.

---

## Main research questions represented in the code

The repository is organized around the following questions:

- Can the reported PWL Chua RC-HAVOK result be reproduced closely from the information available in the base paper?
- Does retaining the identified HAVOK matrices `A` and `B` in floating-point form materially improve reconstruction relative to integer rounding?
- Is reconstruction loss driven more strongly by rounding `A` or rounding `B`?
- Are the conclusions stable across independent reservoir initializations?
- Which tested reservoir/HAVOK hyperparameters produce the largest observed performance variation?
- How sensitive is the fitted RC-HAVOK representation to Gaussian input noise?
- Can smoothing or ridge regularization mitigate the observed noise degradation?
- Does noise-driven growth of the forcing coefficient `B` track reconstruction degradation?
- Are the simulated PWL and cubic source systems consistent with chaotic dynamics under finite-time Lyapunov analysis?
- What do HAVOK-only and ESN-only baselines reveal about the roles of reservoir embedding and HAVOK identification?

---

## Key reproducibility decisions

### 1. PWL Chua parameter-sign discrepancy

The dimensionless PWL Chua model is implemented as

```text
x_dot = alpha * (y - h(x))
y_dot = x - y + z
z_dot = -beta * y
```

with the PWL nonlinearity

```text
h(x) = m1*x + 0.5*(m0 - m1)*(|x+1| - |x-1|)
```

and the project convention

```text
alpha = 9
beta  = 100/7
m0    = -1/7
m1    =  2/7
IC    = (0.1, 0.2, 0.1)
dt    = 0.001 s
```

The reference ISCAS paper prints `m0 = +1/7`. Under the stated equations and initial condition, that sign produces an unbounded trajectory in the project reproduction. The notebooks therefore use `m0 = -1/7` and document this as a **reproducibility discrepancy**, rather than silently treating the printed value as correct.

### 2. Common reservoir baseline

Where a notebook does not explicitly define a study-specific alternative, the principal baseline is centered on:

```text
reservoir neurons : 500
spectral radius   : 0.9
connectivity      : 0.200
leakage           : 0.400
activation        : tanh
input             : [0.1, 0.1*x]
washout           : 2000 samples
HAVOK rank        : 4
sampling interval : 0.001 s
```

These values should **not** be interpreted as one universal protocol for every notebook. Observable choice, Hankel depth, SVD implementation, temporal-mode orientation, integrator, seed count, and evaluation window differ across the executed protocol families and are intentionally preserved.

### 3. Protocol provenance is part of the result

The project evolved from reproduction to extension, ablation, sensitivity screening, exact-SVD confirmation, and canonical noise analysis. The final manuscript therefore treats protocol provenance explicitly rather than retroactively forcing all analyses into one pipeline.

Examples include:

- reservoir PC1 vs. first reservoir component vs. raw `x(t)` as the scalar observable;
- randomized vs. exact/full SVD;
- deterministic temporal-mode orientation only in designated structural-comparison experiments;
- 13 s reconstruction windows vs. final-quarter confirmation windows;
- development-stage cubic evidence vs. later exact-SVD confirmation;
- different Hankel depths in different final protocol families.

When comparing numerical values across notebooks, use each notebook's own printed configuration and the protocol map in the manuscript/supplement.

---

## Repository structure

```text
koopman-rc-havok-chua-reproducibility/
├── README.md
├── requirements.txt
├── SHA256SUMS.txt
├── .gitignore
├── docs/
│   ├── ARCHIVE_AUDIT.md
│   ├── FILE_SELECTION.md
│   ├── NOTEBOOK_INDEX.md
│   └── SOURCE_MANIFEST.csv
└── notebooks/
    ├── 01_core_reproduction/
    │   ├── RC_HAVOK_Base_Reproduction.ipynb
    │   └── RC_HAVOK_Confirmation_Run_FULL.ipynb
    ├── 02_dynamics_extensions/
    │   ├── RC_HAVOK_Cubic_Chua_Extension.ipynb
    │   └── RC_HAVOK_Lyapunov_Exponents.ipynb
    ├── 03_stability_sensitivity/
    │   ├── RC_HAVOK_PWL_Seed_Stability.ipynb
    │   ├── RC_HAVOK_Cubic_Chua_Seed_Stability.ipynb
    │   └── RC_HAVOK_Hyperparameter_Sensitivity_REVIEW_FIXED_executed.ipynb
    ├── 04_noise_robustness_mitigation/
    │   ├── RC_HAVOK_Noise_Robustness_V2_Q1.ipynb
    │   ├── RC_HAVOK_Cubic_Chua_Noise_Robustness.ipynb
    │   ├── RC_HAVOK_Noise_Mitigation_Regularized_Fixed.ipynb
    │   └── RC_HAVOK_BInflation_Spearman.ipynb
    └── 05_ablation_component_baselines/
        ├── RC_HAVOK_AB_Rounding_Ablation_FINAL.ipynb
        ├── RC_HAVOK_ESN_Only_Baseline.ipynb
        └── RC_HAVOK_HAVOK_Only_Baseline.ipynb
```

---

## Notebook guide

| Order | Notebook | Purpose |
|---:|---|---|
| 1 | [`RC_HAVOK_Base_Reproduction.ipynb`](notebooks/01_core_reproduction/RC_HAVOK_Base_Reproduction.ipynb) | Close reproduction of the base PWL Chua Modified/Original RC-HAVOK benchmark. |
| 2 | [`RC_HAVOK_Confirmation_Run_FULL.ipynb`](notebooks/01_core_reproduction/RC_HAVOK_Confirmation_Run_FULL.ipynb) | Full `T_END=200 s`, exact-SVD, 10-seed confirmation of the locked baseline for PWL and cubic Chua. |
| 3 | [`RC_HAVOK_Cubic_Chua_Extension.ipynb`](notebooks/02_dynamics_extensions/RC_HAVOK_Cubic_Chua_Extension.ipynb) | Replaces the PWL diode with the smooth cubic Chua nonlinearity while retaining the development-stage RC-HAVOK pipeline. |
| 4 | [`RC_HAVOK_Lyapunov_Exponents.ipynb`](notebooks/02_dynamics_extensions/RC_HAVOK_Lyapunov_Exponents.ipynb) | Finite-time Lyapunov spectra and Kaplan–Yorke dimension estimates for the PWL and cubic source dynamics. |
| 5 | [`RC_HAVOK_PWL_Seed_Stability.ipynb`](notebooks/03_stability_sensitivity/RC_HAVOK_PWL_Seed_Stability.ipynb) | PWL reproducibility across 20 independent ESN reservoir initializations. |
| 6 | [`RC_HAVOK_Cubic_Chua_Seed_Stability.ipynb`](notebooks/03_stability_sensitivity/RC_HAVOK_Cubic_Chua_Seed_Stability.ipynb) | Development-stage 20-seed cubic stability analysis. |
| 7 | [`RC_HAVOK_Hyperparameter_Sensitivity_REVIEW_FIXED_executed.ipynb`](notebooks/03_stability_sensitivity/RC_HAVOK_Hyperparameter_Sensitivity_REVIEW_FIXED_executed.ipynb) | Executed OFAT screening of reservoir size, spectral radius, connectivity, leakage, and HAVOK rank. |
| 8 | [`RC_HAVOK_Noise_Robustness_V2_Q1.ipynb`](notebooks/04_noise_robustness_mitigation/RC_HAVOK_Noise_Robustness_V2_Q1.ipynb) | PWL Gaussian input-noise sensitivity with 20 realizations per level and statistical summaries. |
| 9 | [`RC_HAVOK_Cubic_Chua_Noise_Robustness.ipynb`](notebooks/04_noise_robustness_mitigation/RC_HAVOK_Cubic_Chua_Noise_Robustness.ipynb) | Cubic Gaussian input-noise sensitivity and comparison against the PWL reference. |
| 10 | [`RC_HAVOK_Noise_Mitigation_Regularized_Fixed.ipynb`](notebooks/04_noise_robustness_mitigation/RC_HAVOK_Noise_Mitigation_Regularized_Fixed.ipynb) | Combined PWL+cubic denoising/regularization experiments. |
| 11 | [`RC_HAVOK_BInflation_Spearman.ipynb`](notebooks/04_noise_robustness_mitigation/RC_HAVOK_BInflation_Spearman.ipynb) | Spearman analysis of noise level, forcing-coefficient growth, `R²`, and RMSE. |
| 12 | [`RC_HAVOK_AB_Rounding_Ablation_FINAL.ipynb`](notebooks/05_ablation_component_baselines/RC_HAVOK_AB_Rounding_Ablation_FINAL.ipynb) | Four-case float/rounded `A`/`B` ablation across matched seeds. |
| 13 | [`RC_HAVOK_ESN_Only_Baseline.ipynb`](notebooks/05_ablation_component_baselines/RC_HAVOK_ESN_Only_Baseline.ipynb) | Closed-loop ESN-only negative-control baseline on raw `x(t)`. |
| 14 | [`RC_HAVOK_HAVOK_Only_Baseline.ipynb`](notebooks/05_ablation_component_baselines/RC_HAVOK_HAVOK_Only_Baseline.ipynb) | HAVOK applied directly to raw Chua `x(t)` to isolate the reservoir-embedding contribution. |

For the manuscript/supplement mapping, see the section below and [`docs/NOTEBOOK_INDEX.md`](docs/NOTEBOOK_INDEX.md).

---

## Mapping to the no-code Supplementary Material

The code repository is the implementation companion to the supplementary sections:

| Supplementary section | Repository implementation |
|---|---|
| S2 — system verification, published-result reproduction, cubic extension | Base reproduction + cubic extension notebooks |
| S3 — A/B coefficient-rounding ablation | `RC_HAVOK_AB_Rounding_Ablation_FINAL.ipynb` |
| S4 — canonical noise robustness, mitigation, and statistics | Noise robustness, cubic robustness, mitigation, and B-inflation notebooks |
| S5 — reservoir seed stability | PWL and cubic seed-stability notebooks |
| S6 — hyperparameter sensitivity | Executed v3 OFAT notebook |
| S7 — finite-time Lyapunov spectrum | Lyapunov notebook |
| S8 — diagnostic HAVOK-only and ESN-only baselines | HAVOK-only and ESN-only notebooks |
| S9 — exact-SVD 200-s confirmation | Full confirmation notebook |

The supplementary document intentionally omits source-code listings; this repository is the code-side archival companion.

---

## Selected validated results

These values are provided to make it easier to confirm that a rerun is aligned with the manuscript record. They should always be interpreted under the relevant notebook protocol.

### Base-paper reproduction

| Model | ISCAS 2025 target | This reproduction |
|---|---:|---:|
| Modified RC-HAVOK `R²` | 0.99428 | 0.98715 |
| Modified RC-HAVOK RMSE | `3 × 10⁻⁴` | `2.50 × 10⁻⁴` |
| Original RC-HAVOK `R²` | 0.15322 | 0.15387 |
| Original RC-HAVOK RMSE | `5 × 10⁻³` | `2.03 × 10⁻³` |

This is reported as a **close reproduction**, not an exact code replication of the base study.

### Exact-SVD 200-s confirmation

| System | Modified median `R²` | Modified mean ± SD | Median RMSE | Original mean `R²` | Modified divergence |
|---|---:|---:|---:|---:|---:|
| PWL | 0.97589 | 0.97460 ± 0.00548 | `3.57 × 10⁻⁴` | −1.642 | 0/10 |
| Cubic | 0.99370 | 0.99397 ± 0.00049 | `1.81 × 10⁻⁴` | −0.53853 | 0/10 |

### A/B coefficient-rounding ablation

Across 10 matched seeds per system, the A-rounded-only reconstruction was lower than the B-rounded-only reconstruction for **all 10 seeds in both systems**. The exact two-sided Wilcoxon test gave `W = 0`, `p = 0.001953` for each system; Holm adjustment across the two system-level tests gave `p_adj = 0.003906`.

The mean dominant-eigenfrequency shifts associated with A-rounding were approximately:

- PWL: `0.392 rad/s` → `5.10 rad` accumulated phase slip over 13 s ≈ `0.81 cycles`
- Cubic: `0.491 rad/s` → `6.39 rad` accumulated phase slip over 13 s ≈ `1.02 cycles`

These values support an oscillatory-misalignment interpretation, but they are not presented as a complete causal decomposition of reconstruction error.

### Seed stability and sensitivity

- PWL 20-seed Modified RC-HAVOK coefficient of variation: **0.384%**.
- The cubic 20-seed notebook belongs to the earlier development-stage PC1/randomized-SVD pipeline and should not be treated as a protocol-matched replicate of the PWL exact-SVD study.
- Later 10-seed exact-SVD confirmation provides the protocol-current cubic reproducibility check.
- In the executed OFAT study, **HAVOK rank produced the largest observed performance variation over the tested ranges**. This statement is intentionally limited to the tested OFAT grids; parameter interactions were not estimated.
- Low leakage (`γ = 0.1`) was particularly problematic for the PWL system; reservoir size, spectral radius, and connectivity were comparatively flatter over their tested ranges.

### Canonical Gaussian-noise study

The final canonical analysis uses one fixed reservoir, exact SVD, and 20 independent realizations at each nonzero Gaussian-noise level.

- Method-level **mean and median `R²` were negative for both systems at every nonzero noise level**.
- No formal divergence occurred in the canonical record, even though poor/negative `R²` was common. Negative `R²` is therefore not treated as synonymous with numerical divergence.
- Moving-average preprocessing provided the most consistent paired low-noise improvement, but it did **not** restore positive method-level mean or median `R²` under the reported noisy conditions.
- Forcing-coefficient magnitude increases with noise, but the final analysis treats `||B||` as a **noise-sensitive, regime-dependent diagnostic**, not a universal scalar predictor of reconstruction quality.

### Finite-time Lyapunov analysis

The finite-time Lyapunov calculation supports chaotic source dynamics for both simulated systems through positive finite-time maximum exponents. The reported Kaplan–Yorke dimensions are approximately:

- PWL: `D_KY = 2.1358` (reported as 2.136)
- Cubic: `D_KY = 2.1102` (reported as 2.110)

The estimates are explicitly treated as finite-time results; rigorous convergence was not established over the 300 s accumulation interval.

### Diagnostic baselines

The ESN-only closed-loop baseline gives median raw-signal `R²` values of approximately:

- PWL: **−2.77784**
- Cubic: **−3.02123**

The HAVOK-only, RC-HAVOK, and ESN-only results are **not direct numerical rankings of the same prediction task**, because they are evaluated in different output/coordinate spaces. They are included as diagnostic controls.

---

## Running the notebooks

### Local environment

The source notebooks use the scientific Python packages listed in [`requirements.txt`](requirements.txt):

```text
numpy
pandas
matplotlib
scipy
scikit-learn
openpyxl
jupyterlab
```

The historical source notebooks did not pin exact package versions. This repository therefore does not invent version numbers that were not recorded in the computational archive.

A typical local setup is:

```bash
python -m venv .venv

# Linux / macOS
source .venv/bin/activate

# Windows PowerShell
# .venv\Scripts\Activate.ps1

pip install -r requirements.txt
jupyter lab
```

The notebooks can also be uploaded to Google Colab.

### Computational cost

Runtime varies substantially. Long-trajectory exact-SVD runs, 20-seed studies, repeated noise realizations, and OFAT sweeps are materially more expensive than the base reproduction notebook. Run the notebooks one at a time and preserve the notebook-specific configuration when reproducing manuscript values.

### Hyperparameter notebook

`RC_HAVOK_Hyperparameter_Sensitivity_REVIEW_FIXED_executed.ipynb` contains its own screening/full-run logic. The final project record treats the **fully executed v3 OFAT configuration** as authoritative. Do not interpret a quick-test configuration as the reported full screening study.

---

## Recommended execution order

For a reviewer or researcher reproducing the study from the beginning:

1. **Base reproduction** — establish the PWL reference result first.
2. **Cubic extension** — verify the smooth-nonlinearity extension under the inherited development protocol.
3. **A/B rounding ablation** — isolate coefficient-precision effects.
4. **PWL and cubic seed studies** — evaluate reservoir-initialization sensitivity with protocol differences kept explicit.
5. **Hyperparameter sensitivity** — inspect the executed OFAT ranges and ranking.
6. **Exact-SVD confirmation** — use the full 200 s / 10-seed confirmation as the primary clean-data reference.
7. **Noise robustness and mitigation** — evaluate Gaussian input-noise sensitivity and mitigation strategies.
8. **B-inflation / Spearman analysis** — inspect the forcing-coefficient diagnostic.
9. **Lyapunov analysis** — characterize the source dynamics independently of RC-HAVOK reconstruction quality.
10. **HAVOK-only and ESN-only baselines** — interpret as diagnostic controls, not directly commensurate forecasting benchmarks.

---

## Generated outputs

Several notebooks generate figures, spreadsheets, NumPy arrays, or pickle files during execution. The GitHub package is intentionally **code-focused**:

- included notebooks are the source of truth for the retained code release;
- embedded outputs already present in the selected source notebooks are preserved;
- standalone generated `.png`, `.xlsx`, `.pkl`, `.npy`, and related run artifacts are ignored by the repository `.gitignore` unless deliberately added later as archival data.

This policy keeps the source repository compact while allowing the notebooks to regenerate experiment outputs.

---

## Source selection and duplicate removal

Two source archives were audited:

- `Notebook.zip` — 20 notebooks; selected as the canonical source because it contained the complete set, including the hyperparameter-sensitivity notebook.
- `Colab Notebooks.zip` — 19 largely duplicate copies, often differing only by embedded execution outputs or trivial notebook metadata.

The final GitHub release keeps one authoritative notebook for each scientifically distinct retained analysis.

Excluded development/superseded files are:

- `RC_HAVOK_AB_Rounding_Ablation_SMOKETEST.ipynb`
- `RC_HAVOK_Confirmation_Run.ipynb`
- `RC_HAVOK_Noise_Mitigation_Cubic_Run.ipynb`
- `RC_HAVOK_Noise_Mitigation_PWL_Run.ipynb`
- `RC_HAVOK_Noise_Mitigation_Regularized.ipynb`
- `RC_HAVOK_Noise_Robustness.ipynb`

The reasons for each exclusion are documented in [`docs/FILE_SELECTION.md`](docs/FILE_SELECTION.md).

---

## Integrity and provenance

The 14 retained notebooks were copied into the repository hierarchy **without editing their internal code** during packaging. Their SHA-256 hashes are recorded in:

- [`SHA256SUMS.txt`](SHA256SUMS.txt)
- [`docs/SOURCE_MANIFEST.csv`](docs/SOURCE_MANIFEST.csv)

On Linux/macOS, verify the packaged notebook hashes from the repository root with:

```bash
sha256sum -c SHA256SUMS.txt
```

On Windows PowerShell, individual files can be checked with:

```powershell
Get-FileHash -Algorithm SHA256 <path-to-file>
```

The archive audit is documented in [`docs/ARCHIVE_AUDIT.md`](docs/ARCHIVE_AUDIT.md).

---

## Reporting conventions used throughout the project

When describing or reusing results from these notebooks, preserve the following distinctions:

- Use **forced reduced-coordinate reconstruction**, not autonomous forecasting, for HAVOK-based `R²`/RMSE.
- Use **supports**, **suggests**, or **is consistent with** where appropriate; avoid turning diagnostic association into causal proof.
- Distinguish **poor/negative `R²`** from **formal numerical divergence**.
- Report median and mean ± SD separately when both are given; do not write “median ± SD”.
- Treat the finite-time Lyapunov exponents and Kaplan–Yorke values as **finite-time estimates**, not rigorously converged invariants.
- Limit OFAT conclusions to the **tested parameter ranges** and do not infer untested parameter interactions.
- Keep development-stage cubic seed-stability evidence distinct from later exact-SVD confirmation.
- Keep HAVOK-only, RC-HAVOK, and ESN-only baseline comparisons within their different coordinate/evaluation spaces.

---

## Data and code availability

The manuscript and no-code Supplementary Material use this repository as the implementation companion. After the public GitHub repository is finalized, the intended archival workflow is:

1. create a versioned GitHub release;
2. archive that release with Zenodo;
3. add the permanent Zenodo DOI to this README and to the manuscript Data Availability Statement;
4. keep the GitHub URL for browsing/current development and use the Zenodo DOI for the immutable cited release.

**Zenodo DOI:** pending first archival release.

---

## Authors and contact

**Md. Zaid Hossain**  
Khulna University of Engineering and Technology (KUET)  
Email: `mdzaideng@gmail.com`

**Borhan Ul Arif**  
Southeast University  
Email: `borhan.ularif@seu.edu.bd`

**Md. Sarowar Hossain Rana** *(corresponding author)*  
Southeast University  
Email: `mdsarowar.rana@seu.edu.bd`

---

## Citation

If you use this repository before the journal article and Zenodo record receive their final bibliographic identifiers, please cite the associated manuscript by title and cite the two foundational RC-HAVOK papers listed under **Scientific context** where relevant.

After the first Zenodo release, this section should be updated with the permanent repository DOI and the final journal citation.

---

## Reproducibility checklist

Before comparing a rerun against a reported value, verify:

- [ ] PWL `m0 = -1/7` is being used for the sign-corrected reproduction.
- [ ] The notebook-specific scalar observable is unchanged.
- [ ] The intended Hankel depth is unchanged.
- [ ] The correct SVD mode (randomized/exact) is being used for that protocol family.
- [ ] The notebook-specific temporal-mode orientation convention is preserved.
- [ ] Reservoir seed(s) and noise realization design match the study being reproduced.
- [ ] The correct evaluation window is used.
- [ ] The HAVOK forcing coordinate is supplied during forced reconstruction.
- [ ] Negative `R²` is not automatically labeled divergence.
- [ ] Cross-notebook comparisons account for protocol and coordinate-space differences.

---

## Documentation

- [`docs/NOTEBOOK_INDEX.md`](docs/NOTEBOOK_INDEX.md) — concise role of each included notebook
- [`docs/FILE_SELECTION.md`](docs/FILE_SELECTION.md) — inclusion/exclusion decisions
- [`docs/ARCHIVE_AUDIT.md`](docs/ARCHIVE_AUDIT.md) — comparison of the two uploaded notebook archives
- [`docs/SOURCE_MANIFEST.csv`](docs/SOURCE_MANIFEST.csv) — provenance, file sizes, cell counts, execution-output status, and SHA-256 hashes

---

### Scope boundary

This repository supports the two **simulated Chua variants** studied here. It does not establish generalization to experimental Chua hardware, unrelated chaotic systems, higher-dimensional dynamics, colored/noise-process families outside the reported tests, or autonomous long-horizon forecasting. Those remain directions for future work.
