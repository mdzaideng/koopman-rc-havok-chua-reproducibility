# Notebook Index

The folders are organized by **scientific role**, not by the date the files were created.

| Folder | Notebook | Research role |
|---|---|---|
| `01_core_reproduction` | `RC_HAVOK_Base_Reproduction.ipynb` | Locked reproduction of the base PWL Chua RC-HAVOK benchmark and Table-II-style performance comparison. |
| `01_core_reproduction` | `RC_HAVOK_Confirmation_Run_FULL.ipynb` | Full reproducible confirmation with `T_END=200 s`, exact full SVD, and multiple reservoir seeds. |
| `02_dynamics_extensions` | `RC_HAVOK_Cubic_Chua_Extension.ipynb` | Smooth cubic Chua nonlinearity extension using the same RC-HAVOK baseline configuration. |
| `02_dynamics_extensions` | `RC_HAVOK_Lyapunov_Exponents.ipynb` | PWL and cubic Chua Lyapunov-exponent / Kaplan–Yorke characterization. |
| `03_stability_sensitivity` | `RC_HAVOK_PWL_Seed_Stability.ipynb` | Reproducibility of the PWL RC-HAVOK result across 20 ESN reservoir initializations. |
| `03_stability_sensitivity` | `RC_HAVOK_Cubic_Chua_Seed_Stability.ipynb` | Seed stability of the cubic Chua extension across independent ESN initializations. |
| `03_stability_sensitivity` | `RC_HAVOK_Hyperparameter_Sensitivity_REVIEW_FIXED_executed.ipynb` | OFAT sensitivity study for reservoir size, spectral radius, connectivity, leakage, and HAVOK rank. |
| `04_noise_robustness_mitigation` | `RC_HAVOK_Noise_Robustness_V2_Q1.ipynb` | PWL Chua Gaussian input-noise robustness with 20 realizations per level and Q1-style statistical reporting. |
| `04_noise_robustness_mitigation` | `RC_HAVOK_Cubic_Chua_Noise_Robustness.ipynb` | Cubic Chua Gaussian input-noise robustness and comparison with the PWL reference. |
| `04_noise_robustness_mitigation` | `RC_HAVOK_Noise_Mitigation_Regularized_Fixed.ipynb` | Combined PWL+cubic evaluation of denoising and ridge-regularization mitigation strategies. |
| `04_noise_robustness_mitigation` | `RC_HAVOK_BInflation_Spearman.ipynb` | Spearman tests linking noise level, forcing-coefficient inflation, R² degradation, and RMSE. |
| `05_ablation_component_baselines` | `RC_HAVOK_AB_Rounding_Ablation_FINAL.ipynb` | Four-case A/B rounding ablation separating A-matrix and B-vector rounding effects. |
| `05_ablation_component_baselines` | `RC_HAVOK_ESN_Only_Baseline.ipynb` | ESN-only closed-loop baseline to isolate the HAVOK model-identification contribution. |
| `05_ablation_component_baselines` | `RC_HAVOK_HAVOK_Only_Baseline.ipynb` | HAVOK-only baseline on raw Chua data to isolate the reservoir embedding contribution. |

## Dependency logic

The notebooks are mostly self-contained and repeatedly define the Chua systems and locked RC-HAVOK parameters for reproducibility. Some later notebooks include reference values from earlier runs for comparison, but the code release does not require the excluded intermediate notebooks.

## Common locked configuration appearing across the project

The supplied notebooks consistently use the sign-corrected PWL Chua configuration around `alpha=9`, `beta=100/7`, `m0=-1/7`, `m1=2/7`, initial condition `(0.1, 0.2, 0.1)`, and a reservoir baseline centered on 500 nodes, spectral radius 0.9, connectivity 0.2, leakage 0.4, and HAVOK rank 4. Individual notebooks explicitly document any changes or study-specific settings.
