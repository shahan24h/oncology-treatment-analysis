# 🚀 Cancer Treatment Pathway Analysis & Risk Prediction

A machine learning pipeline for healthcare risk prediction using CMS Medicare Claims Data on Databricks.

---

## 👤 Author
**Shahan**  
_Data Scientist | ML Engineer_

## 🛠️ Tech Stack
- **Platform**: Databricks (Community Edition)
- **Data Processing**: PySpark, Delta Lake
- **ML Framework**: scikit-learn
- **Experiment Tracking**: MLflow
- **Languages**: Python, SQL

---

## 📚 Project Overview
This project demonstrates an end-to-end ML workflow for predicting high-risk cancer patients using real-world healthcare claims data. All steps adhere to production best practices and clinical relevance, with full experiment tracking and data governance built in.

---

## 💡 Business Problem & Solution
- **Problem**: Healthcare providers need to identify high-risk cancer patients at diagnosis for:
  - Effective resource allocation
  - Early intervention
  - Proactive care management

- **Solution**: Predictive model with **93% accuracy** using only admission-time features for true real-world utility.

---

## 🎯 Key Results
| Metric      | Value    | Interpretation                                                 |
|-------------|----------|--------------------------------------------------------------|
| Accuracy    | 93.15%   | Correctly identifies 93% of patients                         |
| Precision   | 86.84%   | Model is correct 87% of times it predicts high-risk          |
| Recall      | 100%     | Catches **all** high-risk patients (no false negatives)      |
| F1 Score    | 92.96%   | Balanced precision and recall                                |
| ROC AUC     | 88.80%   | Excellent discrimination capability                          |

> **Clinical Impact:** 100% recall means _no high-risk malignant cancer patients are missed_.

---

## 🗺️ Architecture & Pipeline

### Medallion Architecture (Delta Lake)
```mermaid
flowchart TD
    Bronze[Bronze Layer (Raw Data)] --> Silver[Silver Layer (Cleaned Data)] --> Gold[Gold Layer (Model-Ready)]
    Gold --> MLflow[MLFlow Experiment Tracking]
```

- **Bronze Layer**: 116,352 patient records, 66,773 inpatient claims
- **Silver Layer**: 9,851 cancer cases identified, categorized by 11 cancer types, joined with demographics
- **Gold Layer**: 21 engineered features, 3 target variables, admission-time features only (no leakage)
- **MLflow Tracking**: All models, parameters, metrics, and artifacts logged

---

## 📦 Dataset
- **Source**: CMS Medicare Claims Synthetic Public Use Files (DE-SynPUF)
- **Components**:
  - Beneficiary Summary File (demographics/chronic conditions)
  - Inpatient Claims File (admissions/diagnoses/treatments)

- **Cancer Identification**: ICD-9 140-239 codes (Neoplasms)
    - Categories: Malignant, Benign, In Situ, Uncertain
    - Types: Digestive, Respiratory, Lymphatic, etc.

---

## 🏗️ Project Structure
```text
oncology-treatment-analysis/
├── 00_project_config/
│   └── project_config.py          # Config management
├── 01_data_ingestion/
│   └── ingest_cms_data.py         # Load raw data → Bronze
├── 02_data_processing/
│   └── data_cleaning_eda.py       # Clean & ID cancer → Silver
├── 03_feature_engineering/
│   └── feature_creation.py        # Engineer features → Gold
├── 04_model_training/
│   └── model_experiments.py       # Train/track MLflow models
├── 05_model_evaluation/
│   └── [Future: Model comparison]
└── 06_deployment/
    └── [Future: Model deployment]
```

---

## ⚠️ Data Leakage Detection & Resolution
- **Problem**: Unrealistic 100% accuracy caused by outcome-based features (cost, length of stay).
- **Solution**:
  - Removed all outcome/time-dependent variables.
  - Prediction problem redefined to use _only_ admission-time information.
  - Achieved **realistic 93% accuracy** with no leakage.

---

## 🧩 Features Used (No Data Leakage)
| Category      | Features                                     |
|--------------|----------------------------------------------|
| Demographics  | Age, gender, race                            |
| Clinical      | Cancer type, severity, diagnosis count, comorbidity complexity |
| Temporal      | Admission year, month, quarter               |
| History       | Prior claims, patient complexity             |

- ❌ No cost, length of stay, or post-admission features

---

## 🤖 Models Trained
1. **Logistic Regression** (✅ Best Model)
    - Interpretable, production-ready, no overfitting
    - Primary risk screening
2. **Random Forest**
    - 100% accuracy = overfit, unsuitable
3. **Gradient Boosting**
    - Explored as alternative

_All experiments tracked in MLflow_

---

## 🚦 How To Run
1. **Prerequisites**:
    - [ ] Databricks account (Community Edition okay)
    - [ ] Download CMS Medicare synthetic data
2. **Upload Data**:
    - Place CSVs in `/Volumes/workspace/default/file_store`
3. **Run Notebooks** _(in order)_: 
    - `00_project_config/project_config`
    - `01_data_ingestion/ingest_cms_data`
    - `02_data_processing/data_cleaning_eda`
    - `03_feature_engineering/feature_creation`
    - `04_model_training/model_experiments`
4. **View Results**:
    - MLflow UI: `/Users/your-email/oncology-treatment-prediction`
    - Gold layer Delta tables, model performance plots

---

## 🧑‍⚕️ Business Impact
- **Resource Optimization**: Allocate specialists to those who need it most
- **Outcome Improvement**: No malignant cases missed (100% recall)
- **Cost Efficiency**: Proactive intervention = reduced downstream costs
- **Scalability**: Handles 100K+ records. Incremental updates via Delta Lake.

---

## 🚀 Future Roadmap
### Model
- Hyperparameter tuning and cross-validation
- SHAP for explainability
- Predict treatment pathway (multiclass)

### Pipeline
- Automated retraining
- Real-time API inference
- Monitoring dashboard

### Features
- Medication data
- Lab results / vitals
- Time-series analysis

---

## 💎 Key Takeaways
- **ML Best Practices**: No data leakage, model interpretability, experiment tracking
- **Healthcare Domain**: ICD-9 coding, clinical relevance, outcome-first care
- **Engineering Excellence**: Scalable, modular, production-grade architecture

---

## 📬 Contact
**Shahan**  
_Data Scientist | ML Engineer_

_Last updated: December 2024_