#  Network Intrusion Detection System — CIC-IDS Dataset

A machine learning pipeline for detecting network intrusion attacks using the [CIC-IDS dataset](https://www.unb.ca/cic/datasets/ids-2017.html). This project covers the full ML lifecycle: data ingestion, feature engineering, model training, evaluation, and optimization.

---

##  Project Structure

```
├── Data_ignestion.ipynb                    # Data loading, cleaning, and preprocessing
├── FEATURE_ENGINEERING_and_Evaluation.ipynb  # Feature engineering, model training & evaluation
├── Model_Optimization.ipynb               # Hyperparameter tuning and model optimization
└── README.md
```

---

##  Models Trained

| Model | AUC-ROC |
|---|---|
| Logistic Regression | 0.8616 |
| Decision Tree | 0.9466 |
| Random Forest | 0.9865 |
| **Gradient Boosted Trees (GBT)** | **0.9896**  |

> **GBT achieved the best overall performance**, with the lowest false positive rate and highest AUC score.

---

##  Results

### Confusion Matrices
All 4 models were evaluated on the CIC-IDS test set. GBT produced the fewest false positives (607), making it the most reliable for production deployment.

| Model | True Benign | False Positive | False Negative | True Attack |
|---|---|---|---|---|
| Logistic Regression | 427,753 | 3,162 | 81,780 | 37,461 |
| Decision Tree | 428,105 | 2,810 | 6,885 | 112,356 |
| Random Forest | 428,027 | 2,888 | 7,573 | 111,668 |
| **GBT** | **430,308** | **607** | 7,152 | 112,089 |

### ROC Curves
GBT and Random Forest both achieve near-perfect ROC curves (AUC > 0.98), significantly outperforming Logistic Regression.

### Precision-Recall
Decision Tree, Random Forest, and GBT all maintain precision near 1.0 across most recall values — outperforming Logistic Regression significantly in this highly imbalanced dataset.

---

##  Feature Importance

Both Random Forest and GBT agree on the most predictive features:

1. **Flow Duration** — top feature in both models (~0.14–0.16 importance)
2. **Fwd IAT Total** — forward inter-arrival time
3. **Bwd IAT Total** / **Flow IAT Mean**
4. **Idle Mean**, **Active Mean**, **Flow Bytes/s**

> IAT (Inter-Arrival Time) features and flow duration are the strongest predictors of malicious network traffic.

---

##  Pipeline Overview

### 1. Data Ingestion (`Data_ignestion.ipynb`)
- Load and merge raw CIC-IDS CSV files
- Handle missing values and infinite entries
- Encode binary labels (Benign / Attack)

### 2. Feature Engineering & Evaluation (`FEATURE_ENGINEERING_and_Evaluation.ipynb`)
- Feature scaling and selection
- Train/test split with stratification
- Train all 4 classifiers
- Generate confusion matrices, ROC curves, and Precision-Recall curves

### 3. Model Optimization (`Model_Optimization.ipynb`)
- Hyperparameter tuning (Grid Search / Random Search)
- Cross-validation
- Final model selection

---

##  Getting Started

### Prerequisites
```bash
pip install pandas numpy scikit-learn matplotlib seaborn jupyter xgboost
```

### Run the Pipeline
```bash
# Step 1 — Data Ingestion
jupyter notebook Data_ignestion.ipynb

# Step 2 — Feature Engineering & Evaluation
jupyter notebook FEATURE_ENGINEERING_and_Evaluation.ipynb

# Step 3 — Model Optimization
jupyter notebook Model_Optimization.ipynb
```

---

##  Dataset

This project uses the **CIC-IDS 2017 / 2018** dataset from the Canadian Institute for Cybersecurity.

🔗 [Download here](https://www.unb.ca/cic/datasets/ids-2017.html)

> Place the CSV files in a `/data` directory before running the notebooks.

---

## 📈 Key Takeaways

- **GBT is the recommended model** for deployment — best AUC (0.9896) and lowest false positive count
- **Flow Duration and IAT features** are the most discriminative for attack detection
- Logistic Regression underperforms on this task due to non-linear decision boundaries in network traffic data

---

## 📄 License

This project is licensed under the MIT License.
