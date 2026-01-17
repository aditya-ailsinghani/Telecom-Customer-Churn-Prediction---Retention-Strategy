# Telecom-Customer-Churn-Prediction---Retention-Strategy
Personal end-to-end ML project to predict telecom customer churn. Built and evaluated multiple models, used SHAP for interpretability, labeled high-risk customers, assigned retention actions, and designed a Tableau dashboard for targeted churn mitigation.

### Objectives
- Predict customer churn using historical data.
- Understand key factors influencing churn via SHAP.
- Identify high-risk customers.
- Recommend tailored retention strategies.
- Visualize high-risk segments in a Tableau dashboard.

---

## Steps & Pipeline

### 1. **Data Cleaning & Preprocessing**
- Cleaned `TelecomCustomers.csv`
- Fixed types (e.g., `TotalCharges → float`)
- Handled missing values (~11 rows dropped)
- Encoded binary and multi-class categorical features
- Dropped `customerID`

### 2. **Modeling & Evaluation**
Five models were trained to predict customer churn. Each model was evaluated using **precision**, **recall**, **F1-score**, **accuracy**, and **ROC-AUC**:

| Model                  | Accuracy | Precision (Churn) | Recall (Churn) | F1-Score (Churn) | ROC-AUC |
|------------------------|----------|-------------------|----------------|------------------|---------|
| **Logistic Regression** | 80%      | 0.65              | 0.58           | 0.61             | 0.837   |
| **XGBoost**             | 78%      | 0.59              | 0.55           | 0.57             | 0.820   |
| **Weighted XGBoost**    | 75%      | 0.53              | 0.68           | 0.60             | 0.815   |
| **Random Forest**       | 79%      | 0.63              | 0.50           | 0.56             | 0.820   |
| **LightGBM**            | 75%      | 0.52              | 0.76           | 0.62             | 0.833   |

---
### Observations

- **Logistic Regression** gave the best overall performance (balanced accuracy and AUC).
- **LightGBM** achieved the **highest recall (0.76)** for churn, ideal for minimizing false negatives.
- **Weighted XGBoost** improved recall at the expense of precision.
- **Random Forest** delivered stable results across all metrics.
- **Note**: Logistic Regression showed a convergence warning. You can increase `max_iter` or use an alternative solver like `saga`.

---

### 3. **LightGBM Feature Importance**
- Also plotted native LightGBM feature importances (based on split gain):

    
  ![LightGBM Feature Importance](path/to/your/lgbm_importance.png)

- Top features from LightGBM (also reflected in SHAP):
  - `Contract`, `Tenure`, `OnlineSecurity`, `TechSupport`, and `MonthlyCharges`.
  - Reinforces model trustworthiness and business interpretability.

---

### 4. **SHAP Interpretability**
- Used `TreeExplainer` from SHAP on the trained LightGBM model to understand feature contributions.
- Plots generated:
  
  **a. SHAP Summary Bar Plot**  
  Shows average absolute SHAP values across all test samples, ranking global importance.

   
  ![SHAP Bar Plot](path/to/your/shap_bar_plot.png)

  **b. SHAP Summary Beeswarm Plot**  
  Visualizes both feature importance and value effect on churn direction.

    
  ![SHAP Beeswarm](path/to/your/shap_beeswarm.png)

- Key findings from SHAP:
  - `Contract (Month-to-month)` has strongest positive influence on churn.
  - `Tenure` negatively correlates — longer tenure = lower churn risk.
  - Lack of `OnlineSecurity` and `TechSupport` is strongly associated with churn.

---

### 5. **High-Risk Customer Segmentation**
- Computed churn probabilities on test set using the best model (Logistic Regression).
- Customers with churn probability > 0.65 were flagged as **High Risk**.
- Extracted these for downstream retention action planning.

---

### 6. **Retention Strategy Assignment**
- Based on SHAP + domain insights, assigned strategies to reduce churn:
  - Customers on `Month-to-month` contracts → Offered longer-term discounts.
  - No `OnlineSecurity` or `TechSupport` → Bundled plans recommended.
  - Low tenure → Loyalty incentives or personalized onboarding.

- Final dataset with churn scores and assigned actions saved to:  
  `dashboard_data.csv`

---

### 7. **Interactive Tableau Dashboard**
- Built an interactive Tableau dashboard to focus only on **High Risk customers**:
  - **Retention Action Distribution** (bar chart)
  - **Churn Rate by Strategy** (side-by-side bar chart)
  - **Customer Breakdown** (dropdown by retention strategy)
  - **Tenure vs MonthlyCharges** visual by strategy



