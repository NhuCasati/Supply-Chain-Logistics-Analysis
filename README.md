# Supply Chain Logistics Performance Analysis

An end-to-end Python analysis of logistics performance, delivery-time deviation, and operational risk.

## Project summary

This project analyses 32,065 hourly logistics observations recorded from January 2021 to August 2024. It covers data validation, cleaning, feature engineering, exploratory analysis, relationship analysis, and a leakage-aware baseline classification model.

The project demonstrates an important analytical result: a technically clean dataset does not necessarily contain enough signal for reliable prediction or operational recommendations.

## Business question

**Which operational, environmental, warehouse, and transportation factors are associated with delivery-time deviation and logistics risk?**

## Dataset

- **Source:** [Dynamic Supply Chain Logistics Dataset on Kaggle](https://www.kaggle.com/datasets/datasetengineer/logistics-and-supply-chain-dataset/versions/1)
- **Licence:** CC0 — Public Domain
- **File:** `dynamic_supply_chain_logistics_dataset.csv`
- **Rows:** 32,065
- **Columns:** 26
- **Time period:** 2021-01-01 to 2024-08-29
- **Granularity:** Hourly observations

The variables cover vehicle location, fuel consumption, ETA variation, traffic, warehouse inventory, loading time, equipment availability, weather, port congestion, cost, supplier reliability, lead time, IoT monitoring, route risk, customs clearance, driver behaviour, fatigue, disruption likelihood, delay probability, risk classification, and delivery-time deviation.

## Tools

- Python
- pandas
- NumPy
- Matplotlib
- scikit-learn
- Jupyter Notebook

## Analysis workflow

1. Load and validate the expected schema.
2. Audit missing values, duplicates, data types, categories, and numeric ranges.
3. Convert data types and standardise risk categories.
4. Create time, delay, supplier-reliability, and cost-band features.
5. Analyse KPIs, monthly trends, weekdays, risk classes, supplier reliability, and shipping cost.
6. Measure correlations with delivery-time deviation.
7. Build a chronological, leakage-aware random-forest baseline.
8. Translate the evidence into limitations and recommendations.

## Key results

| Result | Value |
|---|---:|
| Total observations | 32,065 |
| Missing values | 0 |
| Exact duplicate rows | 0 |
| Average delivery deviation | 5.18 hours |
| Median delivery deviation | 6.11 hours |
| 90th-percentile delivery deviation | 9.92 hours |
| Average lead time | 5.23 days |
| High-risk share | 74.67% |
| Relative high-delay share | 25.00% |

## Main findings

### 1. Risk classification does not distinguish delivery performance

Average delivery deviation is 5.12 hours for Low Risk, 5.18 hours for Moderate Risk, and 5.18 hours for High Risk. The group distributions overlap extensively.

The supplied risk label should not be used as a proxy for delivery-delay severity without clarification of how it was created.

### 2. No original numeric variable has a meaningful linear relationship with delivery deviation

The apparent strongest correlation belongs to `high_delay_flag`, but that variable is derived directly from delivery-time deviation. Correlations for the original operational variables are close to zero.

This means the current analysis does not support a claim that congestion, weather, route risk, supplier reliability, cost, or customs time individually explains delivery deviation.

### 3. Supplier reliability and shipping cost show no practical separation

Average deviation remains around 5.2 hours across supplier-reliability bands and shipping-cost quartiles. Higher cost is not associated with better delivery performance in the available data.

### 4. The baseline classifier is not operationally useful

The random-forest model predicts every test observation as High Risk.

| Metric | Result |
|---|---:|
| Accuracy | 0.76 |
| High Risk recall | 1.00 |
| Low Risk recall | 0.00 |
| Moderate Risk recall | 0.00 |
| Macro F1 | 0.29 |

The accuracy is misleading because High Risk is the majority class. The model does not detect either minority class.

## Business recommendations

1. Confirm the definitions and generation rules for the supplied risk and delay variables.
2. Establish business-approved delay and severity thresholds.
3. Add shipment, carrier, route, service-level, planned-date, and actual-date fields.
4. Evaluate models against a majority-class baseline using macro F1, balanced accuracy, and minority-class recall.
5. Avoid operational deployment until the target has a clear business definition and measurable relationship with leakage-safe predictors.

## Repository structure

```text
supply-chain-logistics-analysis/
├── Supply_Chain_Logistics_Analysis_Portfolio.ipynb
├── README.md
├── requirements.txt
├── data/
│   ├── dynamic_supply_chain_logistics_dataset.csv
│   └── processed/
│       ├── supply_chain_clean.csv
│       └── data_quality_report.csv
└── reports/
    └── figures/
        ├── monthly_delivery_deviation.png
        ├── risk_class_distribution.png
        ├── delivery_deviation_by_risk.png
        ├── delay_by_weekday.png
        ├── delivery_deviation_correlations.png
        ├── delay_by_supplier_reliability.png
        ├── shipping_cost_vs_delay.png
        └── baseline_confusion_matrix.png
```

## How to run

1. Clone or download the repository.
2. Download the dataset from Kaggle.
3. Place `dynamic_supply_chain_logistics_dataset.csv` inside `data/`.
4. Create and activate a virtual environment.
5. Install the dependencies.
6. Open the notebook and run all cells.

```bash
python -m venv .venv
```

Windows:

```bash
.venv\Scripts\activate
```

macOS/Linux:

```bash
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Start Jupyter:

```bash
jupyter notebook
```

## Limitations

- The dataset may be synthetic or simulated.
- Units and status definitions are not fully documented.
- Several status fields contain continuous values.
- No shipment identifier is available for order-level analysis.
- Carrier, route, service-level, and milestone details are absent.
- Correlation does not establish causality.
- The high-delay flag is a dataset-relative threshold, not an SLA.

## Future work

- Build a formal data dictionary.
- Add route and GPS visualisations.
- Investigate non-linear relationships and interactions.
- Segment observations into operational profiles.
- Compare leakage-safe models using time-based validation.
- Build a dashboard after metric definitions are validated.
