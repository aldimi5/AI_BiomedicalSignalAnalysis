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
