# EHR Missing Data and Uncertainty-Aware Machine Learning

## Project Overview

This project investigates whether machine learning models can account for uncertainty associated with missing information in Electronic Health Record (EHR) data.

The central research question is:

> **Does a model's predictive uncertainty increase when important information is missing from an EHR record, and can that uncertainty help identify predictions that are less reliable?**

The project uses the UCI Diabetes 130-US Hospitals dataset to establish a conventional machine learning baseline and then evaluate how controlled missingness affects predictive performance and model confidence.

The longer-term goal is to determine whether uncertainty information can provide a useful signal for identifying predictions that may require additional review rather than treating imputed values as completely certain observations.

---

## Research Problem

Missing data is common in clinical datasets and can affect the reliability of machine learning predictions. Traditional preprocessing approaches often replace missing values through imputation and then treat the resulting values as ordinary observations.

This project investigates whether explicitly analyzing uncertainty associated with missing clinical information can provide additional insight into model reliability.

The validation experiment will examine whether:

1. introducing controlled missingness in important EHR features affects predictive performance;
2. model confidence changes as relevant information becomes unavailable; and
3. higher uncertainty is associated with a greater likelihood of an incorrect prediction.

---

## Objectives

- Investigate missing-data patterns in a clinical/EHR dataset.
- Establish a conventional missing-data and machine learning baseline.
- Develop and evaluate an uncertainty-aware approach to missing information.
- Compare the uncertainty-aware approach with conventional baseline methods.
- Evaluate predictive performance using appropriate classification metrics.
- Assess the calibration and reliability of model probabilities.
- Examine whether uncertainty can help identify less reliable predictions.
- Quantify uncertainty in reported performance using confidence intervals.

---

## Dataset

The project uses the **UCI Diabetes 130-US Hospitals for Years 1999–2008** dataset.

The dataset contains **101,766 hospital encounters** and 47 predictor variables. The prediction target is the three-class `readmitted` outcome:

- `NO`
- `>30`
- `<30`

The dataset contains substantial missingness across several clinical and administrative variables, making it appropriate for investigating the relationship between missing information and predictive uncertainty.

Patient identifiers are not used as model features. The repository does not commit the underlying clinical dataset to version control.

Dataset documentation and acquisition information are maintained in:

```text
data/README.md
```

---

## Methodology

The current workflow consists of:

1. Dataset acquisition and exploration
2. Missing-data analysis
3. Feature and data-quality analysis
4. Train/test splitting
5. Preprocessing and imputation
6. Baseline predictive modeling
7. Predicted probability and confidence analysis
8. Controlled missingness experiments
9. Uncertainty and error analysis
10. Calibration analysis
11. Statistical validation and confidence intervals
12. Comparison of baseline and uncertainty-aware approaches

### Baseline Models

The current baseline includes:

- Logistic Regression
- Random Forest

The preprocessing pipeline performs missing-value imputation and categorical encoding within the modeling pipeline so that preprocessing parameters are learned from the training data rather than the held-out test set.

The current evaluation uses:

- Accuracy
- Balanced Accuracy
- Macro F1
- Log Loss

Balanced accuracy and Macro F1 are particularly relevant because the three readmission classes are imbalanced. Log loss is also important because the project evaluates predicted probabilities and uncertainty.

---

## Current Baseline Results

The current models are evaluated on a held-out 20% test set using a stratified split.

| Model | Accuracy | Balanced Accuracy | Macro F1 | Log Loss |
|---|---:|---:|---:|---:|
| Logistic Regression | 0.5763 | 0.4011 | 0.3811 | 0.8971 |
| Random Forest | 0.5056 | 0.4586 | 0.4372 | 1.0400 |

These results establish the conventional prediction baseline.

The baseline results do **not** yet establish the project's primary research claim about uncertainty under missingness.

---

## Validation Audit

### Current Validation Status

The current baseline produces predicted class probabilities using `predict_proba()`. An initial confidence measure is calculated using the maximum predicted class probability.

For the Logistic Regression baseline, average confidence is approximately:

- **Correct predictions:** 0.6067
- **Incorrect predictions:** 0.5631

This provides preliminary evidence that model confidence differs between correct and incorrect predictions, but it is not sufficient by itself to establish calibration or demonstrate that uncertainty reliably identifies incorrect predictions.

### Final Validation Plan

The final validation experiment will compare the conventional baseline with the uncertainty-aware approach on the same held-out test observations after introducing controlled EHR missingness.

The analysis will report:

- the primary predictive metric;
- a 95% bootstrap confidence interval for the metric;
- the paired difference between the uncertainty-aware approach and the baseline;
- a 95% confidence interval for that paired difference;
- calibration/reliability results; and
- the relationship between uncertainty and prediction error.

If multiple observations from the same patient are used, resampling and statistical comparisons will account for patient-level grouping rather than treating all encounters as independent observations.

### Decision Rule

An improvement will only be claimed if:

1. the 95% confidence interval for the paired difference excludes zero; and
2. the observed improvement is larger than the smallest gain considered practically meaningful for the intended use.

The final uncertainty-aware validation experiment is planned for **October 30, 2026**.

---

## Leakage and Validation Controls

Several potential leakage paths are considered in the validation process.

### Identifier Leakage

`encounter_id` and `patient_nbr` are excluded from model features because they are identifiers rather than meaningful predictive variables.

### Preprocessing Leakage

Imputation and categorical encoding are performed within the modeling pipeline so that preprocessing parameters are learned from the training data.

### Test-Set Leakage

The held-out test set is reserved for final evaluation. Model development and uncertainty-method decisions should not be repeatedly tuned against final test-set performance.

### Patient-Level Leakage

The dataset contains multiple encounters for some patients. Therefore, a row-level train/test split could potentially place encounters from the same patient in both sets.

A patient-level overlap test will be included in the validation code to ensure that patient identifiers do not overlap between training and test data for the final validation experiment.

---

## Project Status

**Current phase:** Baseline modeling and validation

### Completed

- Initial project scope defined
- Repository structure established
- Python development environment created
- Dependencies documented
- UCI Diabetes 130-US Hospitals dataset selected
- Prediction target finalized as `readmitted`
- Dataset loaded and explored
- Missing-data patterns analyzed
- Feature analysis performed
- Train/test split established
- Preprocessing pipeline implemented
- Logistic Regression baseline implemented
- Random Forest baseline implemented
- Test-set predictions generated
- Baseline performance metrics calculated
- Initial confidence/error analysis performed
- Validation audit requirements identified

### In Progress

- Controlled missingness experiment
- Patient-level leakage test
- Calibration analysis
- Bootstrap confidence intervals
- Paired comparison against the baseline
- Final uncertainty/error analysis

---

## Repository Structure

```text
ehr-uncertainty/
│
├── data/
│   └── README.md              # Dataset documentation and acquisition
│
├── notebooks/
│   └── ...                    # Exploratory analysis and experiments
│
├── src/
│   └── ...                    # Reusable Python source code
│
├── results/
│   └── ...                    # Experimental results, figures, and tables
│
├── docs/
│   └── validation_audit.md    # Week 9/10 validation audit
│
├── .gitignore
├── requirements.txt
└── README.md
```

---

## Environment Setup

The project uses Python and the dependencies listed in `requirements.txt`.

Install the required packages with:

```bash
pip install -r requirements.txt
```

The project was developed using Python 3.12.

---

## Reproducibility

The repository is structured so that datasets are not committed to version control and the required Python dependencies are documented in `requirements.txt`.

The general workflow is:

```text
Dataset
   ↓
Exploratory Analysis
   ↓
Missing-Data Analysis
   ↓
Train/Test Split
   ↓
Preprocessing Pipeline
   ↓
Baseline Models
   ↓
Controlled Missingness
   ↓
Uncertainty Analysis
   ↓
Statistical Validation
   ↓
Final Comparison
```

---

## Project Timeline

### September

- Finalize project scope
- Confirm advisor
- Identify and evaluate candidate datasets
- Finalize prediction task
- Establish baseline methodology

### October

- Complete data preprocessing
- Perform exploratory analysis
- Implement baseline models
- Analyze baseline predictions and confidence
- Develop uncertainty-aware methodology
- Complete validation audit

### November

- Conduct controlled missingness experiments
- Compare baseline and uncertainty-aware approaches
- Perform calibration and uncertainty analysis
- Complete statistical validation
- Analyze experimental results

### December

- Complete final evaluation
- Document findings
- Prepare final project deliverables
- Prepare final presentation

---

## Academic Context

This project is being completed as part of the **University of Florida MS in AI Systems** program.

**Course:** CAI 6826 — Project in AI Systems