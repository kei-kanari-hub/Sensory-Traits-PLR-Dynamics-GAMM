# Sensory Processing Traits and Pupillary Light Reflex Dynamics: GAMM Analysis

This repository contains the dataset and analysis code for the study:  
"Sensory processing traits and the temporal dynamics of the pupillary light reflex in young adults from the general population: A generalized additive mixed model analysis"

## 1. Directory Structure

- `gamm_pipeline_main.R`: R script for the main statistical analysis, including model fitting (M1–M3, S1–S4), autocorrelation correction, and visualization.
- `dataset_gamm_long_3s.csv`: The primary dataset used for the 0–3 s PLR analysis.
- `stimulus_metrics_condition_mean_sd.csv`: Physical metrics (Luminance and Melanopic DER) for each stimulus condition used in Model M3.

## 2. Dataset Overview (`dataset_gamm_long_3s.csv`)

The dataset is provided in a "long" format (100 Hz downsampled). Key variables include:

- Identifiers: `Subject_ID`, `Trial_ID`, `Time_s` (0–3 s).
- Pupil Data: `Pupil_Combined` (Proportional Pupil Change; PPC).
- Traits (z-scored):
  - `LowTh_z`: Dunn’s Low Threshold axis.
  - `Active_z`: Dunn’s Active Strategy axis.
  - `LReg_z`, `SSeek_z`, `SSens_z`, `SAvoid_z`: AASP quadrant scores.
- Covariates: `Sex_bin` (Male=1, Female=0), `age_z`, `AQ_z`, `ASRS_z`, `BDI_z`, `STAI_S_z`, `STAI_T_z`.
- Condition: `CondName` (White, Red, Green, Blue).
- Physical metrics (for M3): `Lv_phys_z` (Luminance), `DER_phys_z` (Melanopic DER).
- AR(1) Support: `AR.start` (Logical flag indicating the start of each trial for autocorrelation modeling).

## 3. Analysis Pipeline

### Preprocessing (Python)
- Downsampling: Original 500 Hz data were aggregated into 10 ms bins (100 Hz) using the mean.
- Exclusion: Participants were included if they provided ≥ 6 valid trials in the White condition.

### Statistical Modeling (R / mgcv)
- Model Framework: Generalized Additive Mixed Models (GAMM) using the `bam()` function with `fREML`.
- Fixed Effects: Non-linear smooths for time, time-by-condition interactions, and time-by-trait tensor product interactions (`ti`).
- Random Effects: Random intercepts and factor-smooth interactions for subjects to account for individual variability in PLR waveforms.
- Autocorrelation: A first-order autoregressive [AR(1)] error structure was implemented within each trial to account for temporal dependencies.
- Inference: Pointwise 95% CIs for predictions and Simultaneous 95% CIs (simulation-based, N=4000) for difference waveforms to control for family-wise error rate.

## 4. How to Reproduce

1. Prerequisites: Ensure R (>= 4.5.2) and the following packages are installed: `mgcv`, `itsadug`, `dplyr`, `ggplot2`, `readr`, `janitor`.
2. Execution:
   - Run `gamm_pipeline_main.R`.
   - The script will automatically estimate the AR(1) rho from an initial null model (M0) and refit the primary models (M1, M2, M3, and S-models).
3. Outputs: Predicted waveforms and statistical summaries will be saved in a time-stamped directory.

## 5. License
This dataset is licensed under a Creative Commons Attribution 4.0 International License (CC BY 4.0). The code is licensed under the MIT License.

## Contact:  
Kei Kanari  
Tohoku University  
Email: kei.kanari.a2@tohoku.ac.jp
