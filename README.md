# diabetes-readmission-prioritization
Healthcare machine-learning prototype for prioritizing post-discharge outreach, using Python and Power BI.


# Diabetes Readmission — Outreach Prioritization

A healthcare analytics and machine-learning project using Python
and Power BI to prioritize post-discharge follow-up.

## Project overview

Hospital outreach teams have limited capacity to contact patients
after discharge. This project explores whether machine learning
can help identify discharge encounters with a higher likelihood
of recorded readmission within 30 days.

The operational question is:

> If a team could prioritize only 10% of eligible discharge
> encounters, how many observed 30-day readmissions would fall
> within that group?

This is a retrospective portfolio prototype. No outreach was
performed, and reduced readmissions or financial savings were
not measured.

## Key results

The logistic regression model ranked 19,773 test discharge
encounters by model score.

| Metric | Result |
|---|---:|
| Test discharge encounters | 19,773 |
| Prioritized encounters | 1,977 |
| Observed readmissions in priority group | 516 |
| Total observed test-set readmissions | 2,244 |
| Readmission rate in priority group | 26.1% |
| Share of all readmissions identified | 23.0% |
| Expected readmissions with random selection | Approximately 224.4 |
| Lift versus random selection | 2.30 |

Prioritizing approximately 10% of test discharge encounters
identified 23.0% of observed 30-day readmissions.

This represents approximately 2.30 times the expected yield
of random selection at the same capacity.

## Dashboard

The Power BI dashboard presents:

- The number of prioritized discharge encounters.
- Observed readmissions within the priority group.
- The expected result under random selection.
- Lift versus random selection.
- Observed readmission rates across score-based tiers.

A dashboard preview is included among the repository files.

## Dataset

Source: UCI Machine Learning Repository —
Diabetes 130-US Hospitals for Years 1999–2008.

The original dataset contains 101,766 hospital encounters
from 130 US hospitals and integrated delivery networks.

Each row represents one hospital encounter, not one unique
patient. A patient may have multiple encounters.

The dataset contains information on demographics, diagnoses,
medications, hospital stays, and prior healthcare utilization.

The analysis includes 99,340 eligible encounters after
excluding death/hospice discharge categories and records
with Unknown/Invalid gender.

## Target definition

The original outcome contains three categories:

- `<30`: recorded readmission within 30 days.
- `>30`: recorded readmission after 30 days.
- `NO`: no recorded readmission.

The binary target, `readmit_30`, is:

- `1` when the original outcome is `<30`.
- `0` when the original outcome is `>30` or `NO`.

The target represents recorded readmission, not whether a
readmission was preventable.

## Methodology

### Data preparation

- Converted `?` placeholders into missing values.
- Removed weight because approximately 97% of values were missing.
- Created a simplified primary-diagnosis group.
- Grouped rare payer and medical-specialty categories using
  frequencies calculated from training data only.
- Selected 19 input features.
- Excluded patient identifiers and outcome columns from model inputs.

### Patient-level split

Encounters were divided into training and test sets using
patient identifiers as groups.

- Training: 79,567 encounters.
- Testing: 19,773 encounters.
- No patients appeared in both sets.

### Preprocessing

The preprocessing pipeline:

- Filled missing numeric values with training medians.
- Scaled numeric features.
- Filled missing categorical values with the most frequent
  training category.
- One-hot encoded categorical features.

### Models

Two models were compared:

1. Logistic regression.
2. Random forest.

Both used balanced class weights.

| Model | Precision at 0.5 | Recall at 0.5 | ROC-AUC |
|---|---:|---:|---:|
| Logistic regression | 0.185 | 0.535 | 0.665 |
| Random forest | 0.178 | 0.607 | 0.670 |

Logistic regression was retained as the simpler baseline
for the outreach-prioritization demonstration.

No statistical test was performed to establish whether
the difference between model results was meaningful.

### Outreach prioritization

Test encounters were ranked by logistic regression score.

The highest-scoring 10% were assigned to the priority group.
The remaining encounters were divided into the next 20%
and the rest.

These tiers represent relative ranking within the test set,
not clinically validated risk thresholds.

## Tools

- Python
- pandas
- scikit-learn
- Jupyter notebook in VS Code
- Power BI
- DAX

## Reproducing the analysis

1. Download the dataset from the UCI Machine Learning Repository.
2. Place `diabetic_data.csv` and `IDS_mapping.csv` in the
   notebook's working directory.
3. Install pandas, scikit-learn, and a Jupyter-compatible environment.
4. Open `Project-Re-admission.ipynb`.
5. Restart the kernel and run all cells from top to bottom.
6. The notebook exports `call_list_demo.csv`.
7. Open the Power BI report, update its CSV source path if needed,
   and refresh the data.

The dashboard export contains selected encounter-level fields
and model outputs without patient identifiers.

## Limitations

- The data are historical and drawn from US hospitals.
- Findings may not generalize to current practice or other countries.
- The queue prioritizes encounters, not necessarily unique patients.
- Model scores have not been assessed for probability calibration.
- The model comparison used the test set; results are exploratory,
  not a fully independent final evaluation.
- No outreach intervention was performed.
- Reduced readmissions and financial savings were not measured.
- Performance differences across demographic groups have not
  been assessed.

## Future improvements

- Compare models using patient-grouped validation within training data.
- Include preprocessing within the validation workflow.
- Assess probability calibration.
- Evaluate subgroup performance.
- Explore uncertainty intervals for evaluation metrics.

## Intended use

This project demonstrates healthcare analytics, machine learning,
and operational prioritization.

It is not intended for clinical deployment.

## Author

Rameen Kanwal
