# Sensory Processing Traits and Pupillary Light Reflex Dynamics: GAMM Analysis

This repository contains the dataset and analysis code for the study:

**“Sensory processing traits and the temporal dynamics of the pupillary light reflex in young adults from the general population: A generalized additive mixed model analysis”**

## Code Update

This archive contains a corrected version of `gamm_pipeline_main.R`.

The correction resolves an execution error that could occur during AR(1) model refitting when locally defined objects were reevaluated outside their original function environment. The corrected script directly refits the AR(1) models using the completed model formula.

This correction affects script execution only. The dataset, model specification, statistical results, figures, interpretations, and conclusions of the published article are unchanged.

## 1. Directory Structure

- `gamm_pipeline_main.R`: Main R script for model fitting, AR(1) autocorrelation correction, statistical inference, and visualization for Models M1–M3 and Supplementary Models S1–S4.
- `dataset_gamm_long_3s.csv`: Primary dataset used for the 0–3 s pupillary light reflex analysis.
- `stimulus_metrics_condition_mean_sd.csv`: Physical stimulus metrics, including luminance and melanopic daylight efficacy ratio, used in Model M3.

## 2. Dataset Overview

### `dataset_gamm_long_3s.csv`

The dataset is provided in long format and was downsampled to 100 Hz. The main variables are described below.

### Identifiers

- `Subject_ID`: Participant identifier.
- `Trial_ID`: Trial identifier.
- `Time_s`: Time from stimulus onset, ranging from 0 to 3 s.

### Pupil Data

- `Pupil_Combined`: Proportional pupil change.

### Sensory-Processing Traits

All trait variables are expressed as standardized z-scores.

- `LowTh_z`: Dunn’s Low Threshold axis.
- `Active_z`: Dunn’s Active Strategy axis.
- `LReg_z`: Low Registration score.
- `SSeek_z`: Sensation Seeking score.
- `SSens_z`: Sensory Sensitivity score.
- `SAvoid_z`: Sensation Avoiding score.

### Covariates

- `Sex_bin`: Sex coded as Male = 1 and Female = 0.
- `age_z`: Standardized age.
- `AQ_z`: Standardized Autism-Spectrum Quotient score.
- `ASRS_z`: Standardized Adult ADHD Self-Report Scale score.
- `BDI_z`: Standardized Beck Depression Inventory score.
- `STAI_S_z`: Standardized State Anxiety score.
- `STAI_T_z`: Standardized Trait Anxiety score.

### Experimental Condition

- `CondName`: Stimulus color condition (`White`, `Red`, `Green`, or `Blue`).

### Physical Stimulus Metrics

These variables are used in Model M3.

- `Lv_phys_z`: Standardized luminance.
- `DER_phys_z`: Standardized melanopic daylight efficacy ratio.

### AR(1) Support

- `AR.start`: Logical indicator marking the beginning of each trial for within-trial AR(1) autocorrelation modeling.

## 3. Analysis Pipeline

### Preprocessing

Preprocessing was performed in Python.

- The original 500 Hz pupil data were aggregated into 10 ms bins using the mean, resulting in a sampling rate of 100 Hz.
- Participants were included if they had at least six valid trials in the White condition.

### Statistical Modeling

Statistical analyses were performed in R using the `mgcv` package.

- Generalized additive mixed models were fitted using `mgcv::bam()` with fast restricted maximum likelihood estimation (`method = "fREML"`).
- Fixed effects included:
  - A nonlinear smooth of time.
  - Time-by-condition smooth interactions.
  - Time-by-trait tensor-product interactions implemented using `ti()`.
- Random effects included:
  - Subject-specific random intercepts.
  - Subject-specific factor-smooth interactions over time to account for individual differences in pupillary response waveforms.
- A first-order autoregressive error structure, AR(1), was fitted within trials to account for temporal autocorrelation.
- The AR(1) parameter, rho, was estimated from an initial model without AR(1) correction and then used to refit the primary and supplementary models.
- Statistical inference included:
  - Pointwise 95% confidence intervals for predicted waveforms.
  - Simulation-based simultaneous 95% confidence intervals for difference waveforms, based on 4,000 simulations, to control the family-wise error rate.

## 4. How to Reproduce the Analysis

### Prerequisites

Use R version 4.5.2 or later.

The following R packages are required:

- `mgcv`
- `itsadug`
- `dplyr`
- `ggplot2`
- `readr`
- `janitor`
- `nlme`
- `MASS`
- `plotfunctions`

### Execution

Place the following files in the same directory:

- `gamm_pipeline_main.R`
- `dataset_gamm_long_3s.csv`
- `stimulus_metrics_condition_mean_sd.csv`

Set the R working directory to that directory and run:

```r
source("gamm_pipeline_main.R")

The script will:

1. Load and validate the analysis data.
2. Fit an initial model without AR(1) correction.
3. Estimate the AR(1) parameter, rho.
4. Fit Models M1–M3 with AR(1) correction.
5. Fit Supplementary Models S1–S4.
6. Generate model summaries, predicted waveforms, difference waveforms, confidence intervals, and figures.

Outputs

All outputs are saved in a time-stamped output directory created by the script.

The output directory may include:

* Model summaries.
* Estimated AR(1) parameters.
* Predicted pupillary response waveforms.
* Difference waveforms.
* Pointwise and simultaneous confidence intervals.
* Statistical result tables.
* Figures generated from the fitted models.

5. Version Information

Corrected Release

This release corrects the model-refitting procedure in gamm_pipeline_main.R.

In the earlier release, the use of update() could cause objects defined locally during model construction to become unavailable when the stored model call was reevaluated. In the corrected script, the AR(1) models are fitted directly using the completed model formula.

The following elements are unchanged:

* The dataset.
* The analyzed variables.
* The model structure.
* The statistical results.
* The figures.
* The interpretations and conclusions of the published article.

6. License

The dataset is licensed under the Creative Commons Attribution 4.0 International License (CC BY 4.0).

The analysis code is licensed under the MIT License.

Contact

Kei Kanari
Tohoku University
Email: kei.kanari.a2@tohoku.ac.jp
