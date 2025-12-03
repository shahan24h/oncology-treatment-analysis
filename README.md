
Cancer Treatment Pathway Analysis & Risk Prediction
End-to-End ML Pipeline using CMS Medicare Claims Data on Databricks

Author: Shahan
Tech Stack: Databricks, PySpark, Delta Lake, MLflow, scikit-learn

Project Overview
This project demonstrates an end-to-end machine learning pipeline for predicting high-risk cancer patients using real-world healthcare claims data. Built on Databricks with Delta Lake architecture and MLflow experiment tracking, this project showcases production-grade data engineering and machine learning best practices.

Business Problem
Healthcare providers need to identify high-risk cancer patients at the time of diagnosis to:

Allocate resources effectively
Provide early intervention for malignant cases
Improve patient outcomes through proactive care management
Solution
Developed a predictive model that achieves 93% accuracy in identifying high-risk cancer patients using only features available at admission time, ensuring real-world applicability.

Key Results
Metric	Value	Interpretation
Accuracy	93.15%	Correctly identifies 93% of patients
Precision	86.84%	When model predicts high-risk, it's correct 87% of time
Recall	100%	Catches ALL high-risk patients (no false negatives)
F1 Score	92.96%	Strong balance of precision and recall
ROC AUC	88.80%	Excellent discrimination capability
Clinical Significance: 100% recall means no high-risk malignant cancer patients are missed by the model.

Architecture & Pipeline
Delta Lake Medallion Architecture
Bronze Layer (Raw Data)
  ├── 116,352 patient records
  └── 66,773 inpatient claims
      ↓
Silver Layer (Cleaned Data)
  ├── Identified 9,851 cancer cases using ICD-9 codes
  ├── Categorized by cancer type (11 categories)
  └── Joined with patient demographics
      ↓
Gold Layer (Model-Ready Features)
  ├── 21 engineered features
  ├── 3 target variables
  └── No data leakage - admission-time features only
MLflow Experiment Tracking
4 models trained and tracked
Parameters, metrics, and artifacts logged
Model versioning and comparison
Dataset
Source: CMS Medicare Claims Synthetic Public Use Files (DE-SynPUF)

Data Components:

Beneficiary Summary File: Patient demographics, chronic conditions
Inpatient Claims File: Hospital admissions, diagnoses (ICD-9 codes), treatments
Cancer Case Identification:

ICD-9 code ranges: 140-239 (Neoplasms)
Categorized by: Malignant, Benign, In Situ, Uncertain
Cancer types: Digestive, Respiratory, Lymphatic, etc.
Technical Implementation
Technologies Used
Platform: Databricks (Community Edition)
Data Processing: PySpark, Delta Lake
ML Framework: scikit-learn
Experiment Tracking: MLflow
Languages: Python, SQL
Project Structure
oncology-treatment-analysis/
├── 00_project_config/
│   └── project_config.py          # Configuration management
├── 01_data_ingestion/
│   └── ingest_cms_data.py         # Load raw data → Bronze layer
├── 02_data_processing/
│   └── data_cleaning_eda.py       # Clean & identify cancer cases → Silver
├── 03_feature_engineering/
│   └── feature_creation.py        # Engineer features → Gold layer
├── 04_model_training/
│   └── model_experiments.py       # Train & track models with MLflow
├── 05_model_evaluation/
│   └── [Future: Model comparison]
└── 06_deployment/
    └── [Future: Model deployment]
Key Learnings & Problem Solving
Data Leakage Identification & Resolution
Problem Identified: Initial models achieved unrealistic 100% accuracy.

Root Cause: Features included outcome variables:

total_claim_amount, length_of_stay_days (only known after treatment)
These directly revealed the target variable
Solution Implemented:

Redefined prediction problem to use only admission-time features
Removed all outcome-based features (cost, length of stay)
Restructured target variable to is_high_risk_patient (Malignant vs. other)
Achieved realistic 93% accuracy with production-ready model
Impact: Demonstrates strong ML fundamentals and production readiness awareness.

Features Used (No Data Leakage)
All features available at patient admission:

Feature Category	Features	Count
Demographics	Age, gender, race	3
Clinical	Cancer type, severity, diagnosis count, comorbidity complexity	4
Temporal	Admission year, month, quarter	3
Patient History	Prior claims, patient complexity	2
Total		11
Features Removed to Prevent Data Leakage:
❌ Cost-related (outcome variables)
❌ Length of stay (outcome variable)
❌ Treatment completion indicators
🤖 Models Trained
1. Logistic Regression ✅ BEST MODEL
Accuracy: 93.15%
Strengths: Interpretable, production-ready, no overfitting
Use Case: Primary risk screening tool
2. Random Forest
Accuracy: 100% (overfitting detected)
Issue: Perfect scores suggest memorization of training data
Learning: Recognized overfitting; selected simpler model
3. Gradient Boosting
Alternative ensemble method explored
All experiments tracked in MLflow
How to Run This Project
Prerequisites
Databricks account (Community Edition works)
CMS Medicare Synthetic Data downloaded
Steps
Upload Data:

Upload CSV files to /Volumes/workspace/default/file_store
Run Notebooks in Order:

   00_project_config/project_config
   01_data_ingestion/ingest_cms_data
   02_data_processing/data_cleaning_eda
   03_feature_engineering/feature_creation
   04_model_training/model_experiments
View Results:
Check MLflow UI: Experiments → /Users/your-email/oncology-treatment-prediction
Review Delta tables in Gold layer
Examine model performance visualizations 
Business Impact
Healthcare Provider Value
Early Risk Identification: Predict high-risk patients at admission
Resource Optimization: Allocate specialists to high-risk cases
Improved Outcomes: 100% recall ensures no malignant cases missed
Cost Efficiency: Preventive care for identified high-risk patients
Scalability
Pipeline processes 100K+ records efficiently
Delta Lake enables incremental updates
MLflow facilitates model versioning and deployment 
Future Enhancements
Model Improvements:

Hyperparameter tuning with cross-validation
SHAP values for model explainability
Treatment pathway prediction (multiclass)
Pipeline Enhancements:

Automated retraining pipeline
Real-time inference API
Model monitoring dashboard
Additional Features:

Incorporate medication data
Add lab results and vitals
Time-series analysis of treatment progression
Key Takeaways
 Technical Excellence:

Built production-grade ML pipeline with Delta Lake architecture
Implemented proper experiment tracking with MLflow
Demonstrated data leakage identification and resolution
 Healthcare Domain Knowledge:

ICD-9 code processing and cancer categorization
Healthcare claims data analysis
Clinical relevance in model design (100% recall priority)
 ML Best Practices:

Recognized and fixed data leakage
Identified model overfitting
Selected interpretable, production-ready model
Validated with realistic healthcare data
Contact
Shahan
Data Scientist | ML Engineer
Last Updated: December 2024
