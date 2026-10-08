# EHR Missing Data and Uncertainty-Aware Machine Learning

## Project Overview

This project investigates machine learning approaches for handling missing data in Electronic Health Record (EHR) datasets while accounting for uncertainty associated with missing or imputed information.

Essentially, does a model's predictive uncertainty increase when important information is missing from an EHR record, and can that uncertainty help identify predictions that are less reliable?

The project will explore whether incorporating uncertainty into the data processing and predictive modeling pipeline can provide more reliable predictions than conventional approaches that treat imputed values as certain observations.

## Research Problem

Missing data is common in clinical datasets and can affect the reliability of machine learning models. Traditional imputation methods replace missing values with estimated values but may not adequately represent the uncertainty associated with those estimates.

This project will investigate uncertainty-aware approaches to missing clinical data and evaluate their impact on downstream machine learning predictions.

## Objectives

* Investigate missing-data patterns in an accessible clinical/EHR dataset.
* Establish a conventional missing-data and machine learning baseline.
* Develop or evaluate an uncertainty-aware approach to handling missing information.
* Compare the uncertainty-aware approach with appropriate baseline methods.
* Analyze predictive performance and the reliability of model predictions.
* Examine how uncertainty information may help identify less reliable predictions.

## Dataset

The final dataset and prediction outcome are currently being evaluated and will be selected based on dataset accessibility, suitability for the research question, and advisor feedback.

The dataset acquisition plan will be finalized during the initial project and advisor discussions.

No clinical or potentially sensitive datasets will be committed to this repository.

## Methodology

The planned workflow consists of:

1. Dataset acquisition and exploration
2. Missing-data analysis
3. Data preprocessing
4. Baseline imputation and predictive modeling
5. Uncertainty-aware methodology
6. Model evaluation and comparison
7. Uncertainty and error analysis
8. Documentation of findings

The specific uncertainty methodology and prediction task will be finalized after dataset selection and advisor feedback.

## Project Status

**Current phase:** Project setup and research planning

Completed:

* Initial project scope defined
* Local development environment created
* Python dependencies documented
* Repository structure established
* Advisor outreach initiated

In progress:

* Advisor matching and initial advisor touchpoint
* Dataset identification and access planning
* Finalization of prediction task and methodology

## Repository Structure

```text
ehr-uncertainty-ml/
│
├── data/          # Dataset documentation and locally stored data
├── notebooks/     # Exploratory analysis and experiments
├── src/           # Reusable Python source code
├── results/       # Experimental results, figures, and tables
├── docs/          # Project documentation
├── .gitignore     # Files excluded from version control
├── requirements.txt
└── README.md
```

## Environment Setup

The project uses Python and the dependencies listed in `requirements.txt`.

To install the required packages:

```bash
pip install -r requirements.txt
```

## Project Timeline

### September

* Finalize project scope
* Confirm advisor
* Identify and evaluate candidate datasets
* Finalize prediction task
* Establish baseline methodology

### October

* Complete data preprocessing
* Perform exploratory analysis
* Implement baseline models
* Develop uncertainty-aware methodology

### November

* Conduct experiments
* Compare baseline and uncertainty-aware approaches
* Perform error and uncertainty analysis
* Finalize experimental results

### December

* Complete final evaluation
* Document findings
* Prepare final project deliverables and presentation

## Academic Context

This project is being completed as part of the University of Florida MS in AI Systems program.
