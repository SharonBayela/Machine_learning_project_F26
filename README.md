# Health Insurance Claim Severity and Frequency Analysis

**Frequency-Severity Modeling for Insurance Claims: A Predictive Framework for Risk-Based Pricing**

This project asks whether claim frequency and severity follow predictable patterns based on patient and policy characteristics, and whether modeling them together can improve risk-based pricing and early fraud detection in health insurance. It combines claim-level healthcare records with public premium and spending data, then applies machine learning methods to explore and predict claim outcomes.

- **Project website:** https://sharonbayela.github.io/
- **Analysis notebook:** [My_project_ML.ipynb](My_project_ML.ipynb)

## Project goals

- Compare claim severity (`Claim_Amount`) and claim frequency (monthly claims per provider).
- Explore differences by insurance type, patient age, chronic-condition status, fraud status, and state.
- Add private, Medicare, and Medicaid premium/spending data for pricing context.
- Build and compare machine learning models for fraud detection, claim severity, and claim frequency.

## Models by task

| Task | Target | Models |
| --- | --- | --- |
| Fraud detection (classification) | `Is_Fraud` | Logistic Regression, Naive Bayes, Decision Tree, Random Forest, AdaBoost, Gradient Boosting, XGBoost |
| Claim severity (regression) | `Claim_Amount` | Linear Regression, Decision Tree, Random Forest, AdaBoost, Gradient Boosting, XGBoost |
| Claim frequency (count or claim/no claim) | `No_of_Claims_Provider` | Linear Regression (baseline), Logistic Regression (binary version), Random Forest, Gradient Boosting, XGBoost |
| Data exploration (unsupervised) | None | Clustering (K-Means, hierarchical), Principal Component Analysis (PCA) |

The expected pure premium combines the two risk components: predicted frequency × predicted severity.

### Status

| Method | Status |
| --- | --- |
| Clustering | Complete |
| PCA | Complete |
| Naive Bayes | Complete |
| Decision Tree | Complete |
| Regression (linear and logistic) | Planned |
| AdaBoost | Planned |
| Gradient Boosting | Planned |
| XGBoost | Planned |
| Random Forest | Planned |

## Data sources

| Dataset | Purpose | Source |
| --- | --- | --- |
| `healthcare_fraud_detection.csv` | Claim-level healthcare and fraud data (10,000 claims). | [Kaggle: Healthcare Fraud Detection Dataset](https://www.kaggle.com/datasets/nudratabbas/healthcare-fraud-detection-dataset) |
| `private_premiums.csv` | State-level Marketplace benchmark premiums. | [KFF Marketplace Average Monthly Benchmark Premiums](https://www.kff.org/state-indicator/?s=Marketplace+Average+Monthly+Benchmark+Premiums) |
| `medicare_premiums.csv` | Traditional Medicare enrollment and payments per enrollee by state. | [KFF Medicare spending per beneficiary](https://www.kff.org/medicare/state-indicator/per-beneficiary/) |
| `medicaid-premiums.csv` | Medicaid spending per full-benefit enrollee by state and eligibility group. | [KFF Medicaid spending per full-benefit enrollee](https://www.kff.org/medicaid/state-indicator/medicaid-spending-per-full-benefit-enrollee/) |

The premium datasets are state-level and are merged with the claims by state. The merged modeling dataset (`health_data.csv`) has 10,000 records and 23 variables. About 8.3% of claims are labeled fraudulent.

## Key variables

| Concept | Field |
| --- | --- |
| Claim severity | `Claim_Amount` |
| Claim frequency | `No_of_Claims_Provider` (renamed from `Number_of_Claims_Per_Provider_Monthly`) |
| Premium/spending context | `Premiums` |
| Fraud indicator | `Is_Fraud` |
| Chronic-condition indicator | `Chronic_Condition_Flag` |
| Service-to-claim delay | `Service_Claim_days` |

## Project workflow

1. Load and clean the four datasets, standardize state names, and convert Medicare and Medicaid figures to monthly values.
2. Merge the premium data into the claims and create a `Premiums` field.
3. Explore claim frequency, severity, premiums, chronic conditions, fraud, and geography.
4. Apply clustering and PCA to explore structure in the claims data.
5. Split the labeled data 80/20 (stratified, `random_state=42`) and fit and compare supervised models.

## Run the notebook

Use Python 3.9+ and keep the four CSV files in the project root next to the notebook.

```bash
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook My_project_ML.ipynb
```

Run the cells from top to bottom. Charts are saved as PNG files in the project root.

## Notes and limitations

- State-level premium and spending measures provide context and are not an individual member's exact premium.
- Medicare and Medicaid definitions and reporting periods differ, so comparisons should be interpreted carefully.
- The claims dataset may be simulated or synthetic, so patterns may be cleaner than in real claims. Models should be validated on real data before operational use.
