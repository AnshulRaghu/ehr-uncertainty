# Dataset

## Diabetes 130-US Hospitals for Years 1999-2008

This project uses the **Diabetes 130-US Hospitals for Years 1999-2008** dataset from the UCI Machine Learning Repository.

* **UCI Dataset:** Diabetes 130-US Hospitals for Years 1999-2008
* **UCI Dataset ID:** 296
* **Source:** UCI Machine Learning Repository
* **Original data:** 101,766 hospital encounters
* **Features:** 47
* **Target:** `readmitted`

The dataset contains clinical, demographic, administrative, medication, and hospital encounter information for patients with diabetes.

## Data Acquisition

The dataset can be obtained programmatically using the `ucimlrepo` Python package:

```python
from ucimlrepo import fetch_ucirepo

diabetes = fetch_ucirepo(id=296)

X = diabetes.data.features
y = diabetes.data.targets
```

The initial data-audit notebook uses this method to retrieve the dataset and creates a local copy for subsequent analysis.

## Local Data Storage

After the initial acquisition, the dataset is stored locally as:

```text
data/
└── raw/
    └── diabetic_data.csv
```

The notebook can then load the local copy directly:

```python
import pandas as pd

df = pd.read_csv("../data/raw/diabetic_data.csv")
```

This avoids repeatedly downloading the dataset during weekly development while preserving a reproducible acquisition process.

## Repository Policy

The raw dataset is **not committed to this GitHub repository**. This keeps the repository lightweight and avoids redistributing the original dataset unnecessarily.

The repository therefore contains:

* Dataset source information
* Data acquisition instructions
* Data-processing and analysis code
* Documentation describing the expected local file structure

A researcher cloning this repository can reproduce the local dataset using the UCI source and the acquisition procedure documented above.

## Dataset Structure

The dataset contains 47 predictor variables and one target variable:

```text
Predictors: 47
Target:     readmitted
```

The `readmitted` target contains three original categories:

```text
<30    Readmitted within 30 days
>30    Readmitted after 30 days
NO     Not readmitted
```

Any transformation of this target for modeling will be documented in the corresponding analysis notebook.

## Citation

If using this dataset, please cite the original UCI Machine Learning Repository dataset and its associated publication.

**UCI Machine Learning Repository:**
Diabetes 130-US Hospitals for Years 1999-2008. Dataset ID 296.

**Original publication:**

Strack, B., DeShazo, J. P., Gennings, C., Olmo, J. L., Ventura, S., Cios, K., & Clore, J. N. (2014). Impact of HbA1c Measurement on Hospital Readmission Rates: Analysis of 70,000 Clinical Datab