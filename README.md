# Association Rule Mining for Diabetes Hospital Readmission Patterns

This project applies association rule mining to a healthcare dataset to identify patterns associated with diabetic patient readmission within 30 days.

Instead of using association rules for a traditional retail market-basket problem, this project treats each hospital encounter as a “basket” and patient/hospital attributes as “items.” The goal is to discover interpretable patterns that may support healthcare decision-making, such as discharge planning, follow-up care, medication review, and diabetes monitoring.

## Project Goal

The main question explored in this project was:

> What patient encounter patterns are associated with diabetic patients being readmitted within 30 days?

This project does not attempt to predict readmission or prove medical causation. The focus is on pattern discovery and interpretation.

## Dataset

**Dataset:** Diabetes 130-US Hospitals for Years 1999–2008
**Source:** UCI Machine Learning Repository
**Domain:** Healthcare / Diabetes / Hospital Readmission

The dataset contains diabetic patient hospital encounter records from 130 U.S. hospitals and integrated delivery networks. Each row represents one hospital encounter and includes information such as:

* Hospital stay details
* Lab results
* Medication information
* Prior hospital activity
* Readmission status

## Market-Basket Setup

For association rule mining, the dataset was converted into a market-basket style format.

| Rule Mining Concept | Healthcare Meaning                                        |
| ------------------- | --------------------------------------------------------- |
| Basket              | One hospital encounter                                    |
| Item                | A patient or hospital attribute converted into a category |
| Outcome item        | `readmitted=<30`, meaning readmitted within 30 days       |

Example rule format:

```text
number_inpatient >= 1.5, A1Cresult=None → readmitted=<30
```

In plain language, this means that patients with higher prior inpatient activity and no recorded A1C result were associated with readmission within 30 days.

## Tools Used

* Orange Canvas
* Association Rules add-on
* Frequent Itemsets widget
* Association Rules widget

## Orange Workflow

The Orange workflow used for this project was:

```text
File → Select Columns → Discretize
                         ↙          ↘
              Frequent Itemsets   Association Rules
```

### Workflow Explanation

* **File:** Loaded the diabetes hospital readmission dataset.
* **Select Columns:** Selected useful healthcare-related variables and moved identifiers/code-heavy fields out of the rule-mining process.
* **Discretize:** Converted numeric variables into categorical intervals.
* **Frequent Itemsets:** Explored common item combinations.
* **Association Rules:** Generated final if-then rules connected to early readmission.

## Preprocessing

Preprocessing was an important part of this project because the original dataset was not directly ready for association rule mining.

Main preprocessing steps:

* Selected meaningful healthcare-related columns.
* Moved identifiers and code-heavy fields out of the mining process.
* Removed rare individual medication columns that mostly created unhelpful “No medication” rules.
* Kept broader medication-related fields such as `insulin`, `change`, and `diabetesMed`.
* Discretized numeric variables into three equal-frequency intervals.
* Focused the final rules on `readmitted=<30`.

## Final Rule Settings

| Setting            | Value            |
| ------------------ | ---------------- |
| Consequent filter  | `readmitted=<30` |
| Minimum support    | 2%               |
| Minimum confidence | 20%              |
| Antecedent size    | 1 to 3 items     |
| Consequent size    | 1 item           |
| Rules found        | 10               |

These settings were chosen because stricter support and confidence values did not return useful early-readmission rules. The final settings produced a small, interpretable set of rules.

## Evaluation Metrics

| Metric     | Meaning                                                                |
| ---------- | ---------------------------------------------------------------------- |
| Support    | How common the full rule is in the dataset                             |
| Confidence | How often early readmission appears when the left-side pattern appears |
| Lift       | How much stronger the relationship is compared to random chance        |

A lift value greater than 1 means the association is stronger than random chance.

## Key Results

### Top Rules by Lift

| Rule                                                                           | Support | Confidence |  Lift |
| ------------------------------------------------------------------------------ | ------: | ---------: | ----: |
| `number_inpatient >= 1.5, A1Cresult=None → readmitted=<30`                     |   0.027 |      0.217 | 1.949 |
| `number_inpatient >= 1.5, A1Cresult=None, diabetesMed=Yes → readmitted=<30`    |   0.022 |      0.217 | 1.948 |
| `number_inpatient >= 1.5, max_glu_serum=None, A1Cresult=None → readmitted=<30` |   0.025 |      0.216 | 1.935 |

### Top Rules by Support

| Rule                                                           | Support | Confidence |  Lift |
| -------------------------------------------------------------- | ------: | ---------: | ----: |
| `number_inpatient >= 1.5 → readmitted=<30`                     |   0.031 |      0.214 | 1.918 |
| `number_inpatient >= 1.5, max_glu_serum=None → readmitted=<30` |   0.029 |      0.212 | 1.901 |
| `number_inpatient >= 1.5, A1Cresult=None → readmitted=<30`     |   0.027 |      0.217 | 1.949 |

## Best Overall Rules

The best rules were selected based on simplicity, interpretability, and practical usefulness.

### Best Rule 1

```text
number_inpatient >= 1.5 → readmitted=<30
```

This was the simplest and most common useful rule. It showed that patients with higher prior inpatient activity were more associated with 30-day readmission.

### Best Rule 2

```text
number_inpatient >= 1.5, A1Cresult=None → readmitted=<30
```

This rule had the highest lift and remained easy to interpret. It suggests that patients with prior inpatient activity and no recorded A1C result may be an important group for closer follow-up.

### Best Rule 3

```text
number_inpatient >= 1.5, A1Cresult=None, diabetesMed=Yes → readmitted=<30
```

This rule adds medication context and may support actions such as medication review, diabetes monitoring, and discharge planning.

## Practical Healthcare Value

The value of these rules is not only in the support, confidence, and lift values. The value comes from how these patterns could be used in a healthcare setting.

A hospital analyst could use these patterns as decision-support signals to identify patients who may benefit from:

* Follow-up calls after discharge
* Medication review
* Diabetes education
* Additional discharge planning
* Care coordination

These rules should not be interpreted as medical causes. They show associations in the dataset, not proof that one factor causes readmission.

## Limitations

* Association rules show patterns, not causation.
* The dataset is historical and covers 1999–2008.
* Missing values such as `A1Cresult=None` and `max_glu_serum=None` should be interpreted carefully.
* The confidence values are moderate because early readmission is a smaller outcome group.
* Additional validation would be needed before using these rules in a real clinical decision-making system.

## Repository Contents

```text
.
├── README.md
├── report/
│   └── association_rule_mining_diabetes_readmission_report.docx
├── presentation/
│   └── Association_Rule_Mining_Diabetes_Readmission.pptx
├── screenshots/
│   ├── orange_workflow.png
│   ├── rule_settings.png
│   └── top_rules_lift.png
└── data/
    └── diabetic_data.csv
```

Note: The dataset may need to be downloaded separately from the UCI Machine Learning Repository if it is not included in this repository.
