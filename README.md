<p align="center">
  <img src="https://img.shields.io/badge/R-4.0%2B-276DC3?style=for-the-badge&logo=r&logoColor=white" alt="R 4.0+"/>
  <img src="https://img.shields.io/badge/caret-6.0+-blue?style=for-the-badge" alt="caret"/>
  <img src="https://img.shields.io/badge/tidymodels-1.0+-blueviolet?style=for-the-badge" alt="tidymodels"/>
  <img src="https://img.shields.io/badge/status-stable-success?style=for-the-badge" alt="stable"/>
  <img src="https://img.shields.io/badge/license-MIT-yellow?style=for-the-badge" alt="MIT"/>
</p>

<h1 align="center">🧬 R Machine Learning Workbench</h1>

<p align="center">
  <b>End-to-end R machine learning pipeline — from data to publication-ready results</b><br>
  RF · XGBoost · LASSO · SVM · Feature Selection · Hyperparameter Tuning · CV · ROC · SHAP · Survival ML
</p>

<p align="center">
  🇺🇸 English · <a href="#-快速开始">🇨🇳 中文</a>
</p>

---

## ✨ Features

| Capability | Details |
|------------|---------|
| **Data Preprocessing** | Stratified/random split, missing imputation, scaling, encoding, feature engineering |
| **Feature Selection** | Boruta, Recursive Feature Elimination (RFE), LASSO-based selection |
| **Model Zoo** | Random Forest (`ranger`), XGBoost, LASSO/Elastic Net (`glmnet`), SVM (`kernlab`) |
| **Tuning** | Grid search, random search, Bayesian optimization |
| **Cross-Validation** | k-fold, Repeated CV, LOOCV, Bootstrap |
| **Evaluation** | ROC/AUC, PR curves, calibration plots, confusion matrix, threshold analysis |
| **Interpretability** | SHAP values (DALEX + shapr), variable importance |
| **Survival Analysis** | Random Survival Forest (RF-SRC), Coxnet (LASSO Cox), XGBoost Cox |
| **Reporting** | R Markdown report with embedded tables & figures, publication-ready PDF/PNG plots |

---

## 📥 Installation

### Prerequisites
- **R >= 4.0** ([Download](https://cran.r-project.org/))
- **56 R packages** — all installed automatically

### Windows (PowerShell)
```powershell
# 1. Clone or download this repo
cd r-machine-learning-sanweisg

# 2. Install all dependencies
.\scripts\launcher.ps1 install

# 3. Run the full pipeline
.\scripts\launcher.ps1 pipeline --data your_data.csv --target outcome
```

### Linux / macOS
```bash
bash scripts/launcher.sh install
bash scripts/launcher.sh pipeline --data your_data.csv --target outcome
```

---

## 🚀 Quick Start

### 30-Second Demo with Iris Data
```bash
# Windows
.\scripts\launcher.ps1 pipeline --data iris --target Species --type classification --cv 5
```

```bash
# Linux/macOS
bash scripts/launcher.sh pipeline --data iris --target Species --type classification --cv 5
```

That's it. The workbench handles: data split → feature engineering → model training (RF + XGBoost + LASSO + SVM) → cross-validation → ROC curves → SHAP explanations → comparison → HTML report.

---

## 📋 Full Command Reference

### Windows (PowerShell)
```powershell
# Dependencies
.\scripts\launcher.ps1 install                            # Install 56 R packages

# Data
.\scripts\launcher.ps1 split --data data.csv --target outcome --ratio 0.7 --method stratified
.\scripts\launcher.ps1 explore --data data.csv --target outcome     # EDA with plots
.\scripts\launcher.ps1 feature_engineer --data data.csv --target outcome
.\scripts\launcher.ps1 feature_select --data train.csv --target outcome --method boruta

# Modeling
.\scripts\launcher.ps1 rf --data train.csv --target outcome --cv 5 --tune TRUE
.\scripts\launcher.ps1 xgboost --data train.csv --target outcome --cv 5 --tune TRUE
.\scripts\launcher.ps1 lasso --data train.csv --target outcome --cv 5
.\scripts\launcher.ps1 svm --data train.csv --target outcome --cv 5 --tune TRUE
.\scripts\launcher.ps1 tune --data train.csv --target outcome --model xgboost --method bayesian
.\scripts\launcher.ps1 cv --data data.csv --target outcome --model rf --k 10

# Evaluation & Interpretation
.\scripts\launcher.ps1 compare --data data.csv --target outcome --models rf,xgboost,lasso,svm
.\scripts\launcher.ps1 roc --preds predictions.csv --truth outcome_col.csv --outcome outcome
.\scripts\launcher.ps1 calibration --preds predictions.csv --truth outcome_col.csv --outcome outcome
.\scripts\launcher.ps1 shap --model output/models/model.rds --data test.csv --target outcome

# Survival Analysis
.\scripts\launcher.ps1 survival --data survival_data.csv --time time --event status --model rfsrc

# Full Pipeline & Report
.\scripts\launcher.ps1 pipeline --data data.csv --target outcome --type classification --cv 5 --models rf,xgboost,lasso
.\scripts\launcher.ps1 report --data data.csv --target outcome
```

---

## 📁 Output Structure

```
output/
├── plots/          # Publication-ready plots (PDF + PNG 300 DPI)
│   ├── *_importance.pdf    # Variable importance
│   ├── *_roc_gg.pdf        # ROC curves
│   ├── *_pr.pdf            # Precision-Recall curves
│   ├── *_calibration.pdf   # Calibration curves
│   ├── *_shap.pdf          # SHAP summary beeswarm
│   └── *_km.pdf            # Kaplan-Meier (survival)
├── models/         # Trained model objects (.rds)
├── predictions/    # Predictions (.csv)
├── reports/        # Analysis reports (.html)
└── tables/         # Metrics & results (.csv)
```

---

## 🧪 Supported Data Types

| Task Type | Target Format | Example |
|-----------|--------------|---------|
| Classification | factor (binary/multi) | `Yes` / `No`, `0` / `1` |
| Regression | numeric | Biomarker level, age |
| Survival | time (numeric) + event (0/1) | Overall survival in months |

---

## 🔬 Bioinformatics Use Cases

This workbench was designed with biomedical researchers in mind:

- **Prognostic model building** — LASSO Cox with cross-validation
- **Biomarker discovery** — Boruta + RF variable importance ranking
- **Multi-omics classification** — SVM on methylation/expression data
- **Survival prediction** — Random Survival Forest + concordance index
- **Diagnostic model validation** — ROC curves with bootstrapped CIs
- **Model explainability** — SHAP values for clinical interpretability

---

## 📊 Example Output

| ROC Curve | Variable Importance | SHAP Summary |
|-----------|-------------------|--------------|
| *[AUC: 0.92, 95% CI: 0.88 – 0.96]* | *Top 10 features ranked* | *Feature contribution beeswarm* |

*(Plots are automatically saved to `output/plots/` in both PDF and PNG)*

---

## 🛠️ Script Architecture

```
scripts/
├── utils.R              # Shared utilities (parse_args, save_plot, metrics)
├── install_packages.R   # Automated dependency installer
├── data_split.R         # Stratified/random split with rsample
├── data_explore.R       # Summary stats, correlation, PCA, missing pattern
├── feature_engineering.R # Encoding, scaling, imputation, interactions
├── feature_selection.R  # Boruta, RFE, LASSO-based filtering
├── rf_model.R           # Random Forest (ranger + caret)
├── xgboost_model.R      # XGBoost (xgboost + caret)
├── lasso_model.R        # LASSO / Elastic Net (glmnet)
├── svm_model.R          # SVM (kernlab + caret)
├── hyperparameter_tune.R # Grid / Random / Bayesian search
├── cross_validation.R   # k-fold, RepeatedCV, LOOCV, Bootstrap
├── model_compare.R      # Multi-model comparison & testing
├── roc_analysis.R       # ROC, PR, threshold optimization (pROC + PRROC)
├── calibration.R        # Calibration curves + Hosmer-Lemeshow test
├── shap_analysis.R      # SHAP via DALEX + shapr
├── survival_ml.R        # RF-SRC, Coxnet, XGBoost Cox
├── pipeline.R           # One-click end-to-end pipeline
└── report_generator.R   # R Markdown → HTML/PDF report
```

---

## 📌 Requirements

- **R >= 4.0**
- 56 R packages (auto-installed via `install` command)
- Core dependencies: `caret`, `tidymodels`, `glmnet`, `xgboost`, `ranger`, `kernlab`, `Boruta`, `pROC`, `DALEX`, `randomForestSRC`, `survival`

---

## 📄 License

MIT

---

## ⭐ Support

If this workbench saves you time in your research, please give it a star ⭐ — it helps others discover it too!

<p align="center">
  <sub>Built with ❤️ for the R & bioinformatics community</sub>
</p>
