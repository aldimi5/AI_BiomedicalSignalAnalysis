# Engineering Journal

## AI-Based Biomedical Signal Analysis Platform

---

# Project Overview

## Objective

Develop a data-driven platform for the analysis of biomedical signals using Python and R. The project aims to combine signal processing, exploratory data analysis, machine learning and scientific programming to analyse ECG recordings from publicly available biomedical datasets.

The project serves both as a technical portfolio and as a learning journey, with the objective of understanding every engineering decision rather than only implementing algorithms.

---

# Development Philosophy

This project is conducted following an engineering-driven methodology.

Before implementing any algorithm or machine learning model, every step must answer the following questions:

1. Why is this step necessary?
2. What problem are we trying to solve?
3. Which method is the most appropriate?
4. What can we learn from the obtained results?
5. How does this influence the following development steps?

The objective is not simply to write code, but to understand the complete reasoning behind each technical decision.

---

# Engineering Workflow

The project follows the workflow below:

1. Project initialization
2. Dataset understanding
3. Exploratory Data Analysis (EDA)
4. Data preprocessing
5. Signal processing
6. Feature engineering
7. Machine learning
8. Model evaluation
9. Documentation and conclusions

---

# Step 1 — Project Initialization

## Objective

Establish a professional development environment before starting any data analysis.

## Initial Hypothesis

A well-structured project improves maintainability, reproducibility and collaboration throughout the development process.

## Implementation

The following project components were created:

* GitHub repository
* Python virtual environment
* Project folder structure
* Requirements file
* Initial documentation
* Jupyter Notebook environment

## Decisions

Python was selected as the primary programming language because of its extensive ecosystem for data science, biomedical signal processing and machine learning.

R will be integrated later for statistical analysis and data visualization, reflecting its widespread use in biomedical research and pharmaceutical industries.

## Lessons Learned

A professional project should begin with a reproducible development environment before implementing any functionality.

---

# Step 2 — Dataset Selection

## Objective

Select a biomedical dataset suitable for learning signal processing, exploratory analysis and machine learning.

## Initial Hypothesis

The dataset should satisfy several requirements:

* publicly available
* scientifically validated
* widely used in biomedical research
* suitable for signal processing
* compatible with machine learning applications

## Decision

The MIT-BIH Arrhythmia Dataset was selected.

## Why this dataset?

The MIT-BIH dataset is considered one of the most established benchmark datasets for ECG analysis.

It contains real electrocardiogram recordings representing different heartbeat categories and is extensively used in biomedical engineering, signal processing and artificial intelligence research.

Using such a benchmark dataset increases both the scientific credibility and reproducibility of the project.

## Lessons Learned

Dataset selection is a technical decision that directly influences the relevance and quality of the entire project.

---

# Step 3 — Dataset Understanding

## Objective

Understand what each observation and each variable represent before performing any analysis.

## Questions

* What does one row represent?
* What does one column represent?
* What is the prediction target?
* Why are there 188 columns?

## Observations

Each row corresponds to one heartbeat extracted from an ECG recording.

The first 187 columns contain sampled electrical measurements of the heartbeat.

The final column represents the heartbeat class.

The horizontal axis displayed during visualization corresponds to sample indices rather than physical time because the sampling frequency has not yet been introduced.

## Interpretation

The dataset consists of physiological time-series rather than independent numerical variables.

Understanding the physical meaning of the measurements is essential before applying machine learning algorithms.

## Lessons Learned

Every dataset should first be understood from both a technical and a physical perspective.

---

# Step 4 — Exploratory Data Analysis (Current Stage)

## Objective

Explore the structure of the dataset and identify potential challenges before preprocessing and machine learning.

## Initial Questions

* How are the heartbeat classes distributed?
* Is the dataset balanced?
* Do different heartbeat classes exhibit different signal morphologies?

## Implementation

The following analyses were performed:

* dataset dimensions
* data types
* class identification
* class frequency computation
* visualization of representative ECG signals

## Observations

Five heartbeat classes were identified.

The dataset is highly imbalanced, with normal heartbeats representing the majority of observations.

Representative ECG signals already exhibit noticeable differences in waveform morphology, amplitude and peak characteristics between classes.

## Interpretation

The observed class imbalance reflects realistic clinical scenarios in which normal heartbeats largely outnumber pathological events.

The morphological differences between heartbeat classes suggest that meaningful patterns exist and can potentially be learned by machine learning models.

## Engineering Decision

The next step will focus on deeper exploratory analysis before applying any preprocessing or predictive models.

Understanding the data remains the priority over model development.

## Lessons Learned

Exploratory Data Analysis is not merely a visualization step; it is a decision-making process that guides the entire machine learning pipeline.

---

## Addendum — Dataset Provenance Verification

While validating a value-range assumption during Phase A of the EDA notebook (`02_exploratory_data_analysis.ipynb`), an unverified claim was found in the documentation: that each heartbeat had been normalized independently ("per-beat min-max normalization"). Rather than continue on that assumption, the provenance and preprocessing pipeline of the derived CSV dataset were traced back to the primary source.

**Verified facts (source paper — Kachuee, Fazeli & Sarrafzadeh, *ECG Heartbeat Classification: A Deep Transferable Representation*, arXiv:1805.00794):**

* The original MIT-BIH recordings were sampled at 360 Hz.
* The pipeline used to derive this CSV dataset resamples the signal to 125 Hz before segmenting it into individual beats.
* Amplitude is normalized to [0,1] at the level of each 10-second recording window, *before* individual beats are extracted — not independently per beat.
* Beats are explicitly zero-padded to a fixed length as an intentional design choice of the pipeline, not an artifact of missing data.

**Empirical findings (this repository's own analysis):** the [0,1] bound holds across all values in both CSV files — consistent with the above, but this only confirms the *global bound*, not that each beat independently spans [0,1].

**Remaining uncertainties:** the exact resampling algorithm (360→125 Hz) and the exact procedure used to construct the specific train/test CSV files distributed for this project are not documented in the source reviewed.

## Lessons Learned (Addendum)

An assumption about the preprocessing pipeline was documented as fact before being traced to a primary source. Provenance should be verified against the original methodology whenever a derived dataset is used, rather than inferred from the data's surface properties alone — surface properties (e.g. a [0,1] range) can be consistent with more than one underlying preprocessing procedure.

---

# Step 5 — Exploratory Data Analysis, Phase B: Effective Signal Length and Zero-Padding

## Objective

Characterize the trailing zero-padding in the fixed-length (187-sample) beat representation, and determine whether the resulting effective segment length is associated with heartbeat class — using the corrected provenance understanding from the addendum above.

## Scientific Question

Is the amount of trailing zero-padding associated with heartbeat class, and if so, does that association reflect a dataset-generation artifact rather than a physiological property of the beat?

## Methodology

`effective_length` was defined as an operational proxy (1 + index of the last non-zero sample per row), computed identically for every beat in both files. Overall and per-class descriptive statistics were computed, followed by a Kruskal-Wallis test (chosen over ANOVA due to skewed, unequal-variance class distributions) with epsilon-squared effect size, and a targeted Mann-Whitney U / rank-biserial comparison for the class flagged by the descriptive statistics (Fusion). No preprocessing, feature engineering, or model training was performed.

## Major Findings

* Trailing padding is substantial and near-universal: median effective length ≈108–109 of 187 samples; only ≈0.9% of beats use the full window. Consistent between train and test.
* Fusion beats are a clear structural outlier: much shorter (median 78 vs. 106–120 for other classes) and much more tightly distributed (std ≈13–14 vs. ≈10–39 elsewhere), reproducible in both train and test.
* The global association between class and length is statistically significant but small (Kruskal-Wallis, epsilon-squared ≈0.03 in both splits — class explains ~3% of rank variance). The Fusion-specific association is large (rank-biserial r ≈0.76–0.77; a random Fusion beat is shorter than a random non-Fusion beat ≈88% of the time).
* Supraventricular beats show a secondary signature: an unusually high rate (~11–12%) of beats using the full 187-sample window with no padding at all, versus under 4% for every other class.

## Interpretation

A small global effect size alongside a large, class-specific effect is not a contradiction — it means four of the five classes overlap substantially in effective length, while Fusion (the rarest class) is the outlier driving nearly all of the association. Per the provenance fact established in the addendum above (pre-padding length = 1.2 × the local median R-R interval of the 10-second window, applied per-window, not per-beat), this is best interpreted as a **local rhythm-context signature correlated with when Fusion beats occur**, not as evidence that Fusion beats have an intrinsically shorter physiological duration.

## Limitations

`effective_length` is a proxy, not a verified physiological boundary (a genuine signal value of exactly 0.0 would be indistinguishable from padding). Statistical association does not establish a causal or physiological mechanism. The exact 360→125 Hz resampling algorithm remains undocumented, so any time-scale conversion (125 Hz → ms) describes the representation, not a validated physiological duration.

## Shortcut-Learning Risk

Because effective length separates Fusion from the rest of the dataset well above chance, any model architecture sensitive to padding (directly, e.g. an explicit length feature, or indirectly, e.g. sequence-length-sensitive layers) has a route to detect Fusion beats without learning ECG morphology. This is a real risk given Fusion is simultaneously the rarest class and the one most reachable by this shortcut — a future model's Fusion recall should not be attributed to morphology learning without first checking a trivial length-based baseline.

## Implications for Future Preprocessing / Modeling

* Any Fusion-class performance claim in later phases must be checked against a length-only baseline before being attributed to learned morphology.
* Padding must not be silently discarded or masked without deciding, deliberately, whether that changes the length-related signal identified here.
* This finding does not itself dictate a preprocessing fix (e.g. masking, cropping, or using length as an explicit feature) — that decision is deferred to the preprocessing phase, informed by this evidence.

## Lessons Learned

An apparent data-quality artifact (zero-padding) can carry real, statistically detectable class information without being a physiological signal — and a single omnibus statistic (small global effect size) can hide a large, actionable, class-specific effect. Both the aggregate and the per-class view are necessary before deciding whether something is noise or risk.
