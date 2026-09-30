# Notebooks

This directory contains Jupyter notebooks used for exploratory analysis, experimentation, and evaluation for the EHR uncertainty project.

## Environment

The project uses Python 3.12 and a project-specific virtual environment.

From the repository root:

```bash
python3.12 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

The notebooks can then be launched with:

```bash
jupyter lab
```

## Notebook Organization

### `01_data_audit.ipynb`

Initial audit of the Diabetes 130-US Hospitals dataset.

The notebook examines:

* Dataset dimensions and structure
* Feature data types
* Target distribution
* Missing values
* Categorical unknown/missing values
* High-missingness features
* Duplicate records
* Patient and encounter structure
* Feature cardinality
* Potential data-quality issues
* Potential sources of data leakage
* Initial visualizations and observations

The purpose of this notebook is to understand the dataset before making decisions about preprocessing, modeling, uncertainty estimation, or selective prediction.

## Reproducibility

All notebooks should be executed from the repository root or using paths relative to the notebook location.

The expected project structure is:

```text
ehr-uncertainty/
├── data/
│   ├── raw/
│   │   ├── diabetic_data.csv
│   │   └── README.md
│   └── README.md
├── notebooks/
│   ├── 01_data_audit.ipynb
│   └── README.md
├── results/
├── src/
├── docs/
├── requirements.txt
├── README.md
└── .gitignore
```

The initial dataset is obtained from the UCI Machine Learning Repository. After acquisition, the local CSV is used for subsequent notebook execution so that analysis does not depend on repeatedly downloading the dataset.

## Reproducibility Guidelines

To reproduce the analysis:

1. Clone the repository.
2. Create and activate the Python 3.12 virtual environment.
3. Install the dependencies from `requirements.txt`.
4. Obtain the dataset according to `data/README.md`.
5. Place the local dataset in `data/raw/`.
6. Launch JupyterLab.
7. Run the notebooks in numerical order.

Randomized experiments should use explicit random seeds where appropriate. The project will use fixed random states to make experimental results reproducible.

## Notebook Conventions

Notebooks are intended to document the reasoning and experiments performed during the project.

Where possible:

* Imports are kept near the beginning of the notebook.
* Data transformations are explicitly documented.
* Important assumptions are explained in Markdown cells.
* Figures include descriptive titles and axis labels.
* Intermediate results are not manually entered into later cells.
* Random processes use fixed seeds.
* Modeling decisions are documented before final evaluation.

As the project develops, additional notebooks will be added for preprocessing, baseline models, uncertainty estimation, and selective prediction experiments.