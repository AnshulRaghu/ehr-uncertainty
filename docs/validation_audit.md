# Validation Audit — Week 9/10

**Project:** EHR Missing Data and Uncertainty-Aware Machine Learning  
**Repository:** `ehr-uncertainty`  
**Dataset:** UCI Diabetes 130-US Hospitals  
**Prediction target:** `readmitted`

## Q1 · Claim

The project investigates whether predictive uncertainty increases when important information is missing from an EHR record and whether uncertainty can help identify predictions that are less reliable.

The current system uses the UCI Diabetes 130-US Hospitals dataset, which contains 101,766 hospital encounters and 47 predictor variables. The prediction target is the three-class `readmitted` outcome: `NO`, `>30`, and `<30`.

The current baseline treats this as a three-class classification problem. The baseline models are multinomial Logistic Regression and Random Forest. The primary uncertainty analysis will use predicted class probabilities and maximum predicted probability as an initial confidence measure.

The current baseline test-set results are:

- Logistic Regression: accuracy = 0.5763, balanced accuracy = 0.4011, macro F1 = 0.3811, log loss = 0.8971.
- Random Forest: accuracy = 0.5056, balanced accuracy = 0.4586, macro F1 = 0.4372, log loss = 1.0400.

These numbers establish the conventional prediction baseline, but they do **not yet support the main research claim** about uncertainty under missingness.

## Q2 · Split

The current modeling notebook separates the data using an 80/20 stratified train/test split with `random_state=42`.

The training set is used to fit the models and estimate preprocessing parameters. The test set is held out for evaluation. Stratification preserves approximately the same class proportions between the training and test sets.

The preprocessing pipeline is fit as part of the training pipeline. Numerical missing values are imputed using the training-set median, while categorical missing values are imputed using the training-set most frequent category. Categorical variables are then one-hot encoded.

This split supports the current baseline performance claim because the test observations are not used to fit the model or estimate the preprocessing parameters. However, the final uncertainty-aware experiment should preserve this separation when introducing controlled missingness. Any thresholds or decisions about uncertainty should also be determined without tuning them on the final test set.

## Q3 · Leakage

Potential leakage was considered during the initial data audit and baseline construction.

The main potential leakage paths are:

1. **Identifier leakage:** `encounter_id` and `patient_nbr` could allow the model to learn identifiers rather than clinically meaningful patterns. Both are explicitly removed before modeling.
2. **Preprocessing leakage:** estimating imputation or encoding information using the full dataset could allow test-set information to influence training. The current preprocessing is contained inside the scikit-learn pipeline and is fit using the training data.
3. **Evaluation leakage:** repeatedly adjusting the model or uncertainty procedure based on final test-set performance could cause the test set to become part of model development. The final uncertainty experiment should therefore use a development/validation procedure for tuning before reporting the final test result.
4. **Patient-level leakage:** the dataset contains multiple encounters for some patients. A random row-level split can therefore place encounters from the same patient in both training and test sets. This is an important remaining validation concern because the current split is row-level rather than patient-group-level.

The current notebook removes `patient_nbr` as a predictor, but that does not by itself prevent the same patient from appearing in both splits. Therefore, patient-level overlap should be explicitly tested before the final validation result is claimed.

A leakage test should be added to the repository to assert that patient identifiers do not overlap between training and test sets. The test should be implemented in the baseline modeling/validation code and referenced here once committed.

## Q4 · Metric and uncertainty

The project uses multiple metrics because the three readmission classes are imbalanced.

The current baseline reports:

- Accuracy
- Balanced accuracy
- Macro F1
- Log loss

Macro F1 and balanced accuracy are particularly useful because the `NO` class is substantially more common than `<30`. Log loss is also important because the project studies predicted probabilities and uncertainty rather than only the final predicted class.

The current baseline results are:

| Model | Accuracy | Balanced Accuracy | Macro F1 | Log Loss |
|---|---:|---:|---:|---:|
| Logistic Regression | 0.5763 | 0.4011 | 0.3811 | 0.8971 |
| Random Forest | 0.5056 | 0.4586 | 0.4372 | 1.0400 |

The current repository does **not yet contain the bootstrap confidence-interval calculation or the final paired comparison for the uncertainty-aware experiment**. Therefore, these baseline point estimates should not be presented as evidence that the final uncertainty-aware system beats the baseline.

For the final experiment, I will compare the conventional baseline against the uncertainty-aware approach on the same held-out test observations. I will compute a 95% bootstrap confidence interval for the primary metric and a paired bootstrap confidence interval for the difference between the two methods.

The primary comparison will focus on predictive reliability under controlled missingness. The final analysis will also examine whether confidence/uncertainty is associated with prediction error.

The decision rule is that an improvement will only be claimed if the 95% confidence interval for the paired difference excludes zero and the improvement is larger than a practically meaningful minimum gain.

## Q5 · Trust

The current baseline produces predicted class probabilities using `predict_proba()`. These probabilities are converted into an initial confidence measure by taking the maximum predicted class probability.

The current notebook demonstrates that confidence differs somewhat between correct and incorrect predictions. For Logistic Regression, the average confidence is approximately 0.6067 for correct predictions and 0.5631 for incorrect predictions.

However, this is not sufficient to establish that the probabilities are calibrated. The project has not yet performed a formal calibration analysis such as reliability diagrams or a calibration metric.

Therefore, calibration remains a validation gap.

The final uncertainty experiment should evaluate whether predicted probabilities are calibrated and whether higher uncertainty is meaningfully associated with a higher probability of error.

No LLM grader or human grader is being used to evaluate model outputs. The model is evaluated using programmatic metrics computed against the held-out target labels, so there is currently no separate human/LLM grader that requires validation.

## Q6 · After launch

If this system were used in a clinical decision-support setting, the main things to monitor would include:

- predictive performance over time;
- class-specific performance, especially the relatively rare `<30` readmission class;
- calibration of predicted probabilities;
- the relationship between uncertainty and prediction error;
- the frequency of high-uncertainty predictions;
- changes in the distribution of missing values;
- changes in important clinical features;
- data-quality and preprocessing failures;
- population or hospital differences in model performance.

A major potential failure mode is that the missingness pattern in new clinical data differs from the historical training data. Because several variables in this dataset have very high missingness, changes in why or when information is missing could change model behavior.

Another potential failure is overconfidence: a model could produce a high-confidence prediction even when important clinical information is unavailable. This is particularly relevant to the project's research question.

If deployed clinically, incorrect predictions could contribute to inappropriate risk assessment or missed opportunities for intervention. Patients could therefore be harmed, particularly if the model is used to support decisions for higher-risk readmission cases without appropriate human oversight.

For this reason, uncertainty should be treated as a signal for additional review rather than as a guarantee that a prediction is correct.

## Q7 · Verdict

The biggest current validation gap is that the main uncertainty-aware claim has not yet been experimentally established.

The baseline model has been implemented and evaluated, but the repository does not yet demonstrate that controlled missingness increases predictive uncertainty or that uncertainty can reliably identify incorrect predictions. Calibration and patient-level leakage testing also need to be strengthened before making a final validation claim.

The single most important fix is to **add the controlled-missingness experiment together with an explicit patient-level leakage test and evaluate the resulting uncertainty-aware predictions against the conventional baseline using a paired 95% confidence interval**.

**Target date: October 30, 2026.**

Until that experiment is completed, the project should claim that it has established a conventional baseline and a validation plan, rather than claiming that the uncertainty-aware approach improves clinical prediction.