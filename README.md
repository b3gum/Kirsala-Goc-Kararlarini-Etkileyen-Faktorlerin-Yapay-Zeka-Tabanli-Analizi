# Rural Migration Prediction — Logistic Regression Model for “Prefers Not to Migrate”

This project analyzes survey data on urban livability factors to predict whether individuals **prefer not to migrate to rural areas** (i.e., choose to stay in the city) using **Logistic Regression**.

The workflow includes data preprocessing, scaling, model training, performance evaluation, coefficient interpretation, and visualization of the most influential factors.

---

## 1️ Dataset Overview

- **Source file:** `Yaşanılabilir Bir Kent İçin Önem Sıralaması (1).xlsx`  
- **Sheet name:** `Form Yanıtları 1`  
- **Target column (binary):** `Göç Etmemeyi Tercih Eder` → converted to 0/1  
- **Feature columns:**
  ```
  Ulaşım, Güvenlik, İklim Değişikliğine Duyarlı, Kadınların Sosyalleşebileceği Alanlar,
  Kendine Özgü Kent Dokusu, İyi Belediyecilik, Sağlık, Sosyal Alanlar, Yeşil Alanlar, 
  Altyapı, Erişilebilirlik, Afetlere Karşı Direnç, Ekonomi, 
  Dengeli Şehirleşme (Kır Kent Arasındaki Nüfus Dengesi), 
  Fazla Nüfuslu Şehir, Göç Alan Şehir (Dış ülkelerden daha çok göç alan),
  Göç Alan Şehir (ülke içinden göç alan), Kültürel Miras Alanlarının Varlığı, 
  Çevre Kirliliğinin Olmaması, Eğitim
  ```
If any columns are missing, the script will display them in the terminal.

---

## 2️ Installation

### Requirements
- Python 3.9+
- Libraries: `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `openpyxl`

### Install dependencies
```bash
python -m venv .venv
source .venv/bin/activate   # or .venv\Scripts ctivate on Windows
pip install -U pandas numpy scikit-learn matplotlib openpyxl
```

---

## 3️ How to Run

Make sure the Excel file is in your project folder.  
Update the path if needed:
```python
path = "Yaşanılabilir Bir Kent İçin Önem Sıralaması (1).xlsx"
```
Then run the Python script or execute it in a Jupyter Notebook.

---

## 4️ Pipeline Summary

1. **Clean column names** (strip newlines, spaces).  
2. **Convert to numeric** (`errors='coerce'` → invalid values become NaN).  
3. **Impute missing values** using median per column.  
4. **Train/Test Split:** 75% train / 25% test, with stratified sampling.  
5. **Scale features:** `StandardScaler` (Z-score normalization).  
6. **Train model:** Logistic Regression  
   ```python
   LogisticRegression(max_iter=500, class_weight='balanced', random_state=42)
   ```
7. **Evaluate results:**
   - Accuracy
   - Classification report (Precision / Recall / F1)
   - Confusion matrix
   - 5-Fold Stratified CV mean & std accuracy
8. **Coefficient interpretation:** β, exp(β), and qualitative explanations.  
9. **Plot:** Horizontal bar chart of the top N influential factors.

---

## 5️ Output & Interpretation

### Main metrics
- **Accuracy:** Correct classification rate on test data.  
- **Classification report:** Per-class precision, recall, and F1.  
- **Confusion matrix:** Shows TP / TN / FP / FN.  
- **5-Fold CV accuracy:** More reliable cross-validation metric.

### Coefficients & Odds Ratio
- **β > 0:** Higher score → higher likelihood to **stay in the city** (not migrate).  
- **β < 0:** Higher score → higher likelihood to **migrate to rural areas**.  
- **exp(β):** Odds ratio; multiplicative change in odds per 1-unit increase.  
- **Ranking:** Sorted by |β| (absolute impact).

> Note: Coefficients are based on **standardized variables**, enabling fair comparison of magnitudes.

### Visualization
A horizontal bar plot titled:  
**“Logistic Regression — Top 12 Influential Factors (β)”**  
Displays both direction and relative strength of impact.

---

## 6️ Design Choices

| Component | Reason |
|------------|--------|
| **StandardScaler** | Improves numerical stability and coefficient interpretability. |
| **class_weight='balanced'** | Handles potential class imbalance. |
| **StratifiedKFold(5)** | Maintains class ratios in folds. |
| **random_state=42** | Ensures reproducibility. |

---

## 7️ Limitations

- Small sample size → risk of overfitting.  
- Multicollinearity among features may distort coefficient meaning.  
- Self-reported survey bias.  
- Logistic regression assumes linear relationships between predictors and log-odds.

---

## 8️ Future Improvements

- Add **interaction terms** (e.g., Security × Social Spaces).  
- Compare with **regularized logistic (L1/L2)** and tree-based models.  
- Use **SHAP** or **Permutation Importance** for explainability.  
- Optimize **decision threshold** for F1 or Recall.  
- Experiment with **SMOTE / ADASYN** to balance the dataset.  
- Cross-validate across different demographics or regions.

---

## 9️ Troubleshooting

| Issue | Possible Cause / Fix |
|--------|----------------------|
| `Missing columns [...]` | Ensure column names in Excel match exactly. |
| `ValueError: could not convert string to float` | Non-numeric entries detected — check raw data. |
| No graph displayed | Add `plt.show()` or use Jupyter Notebook. |
| Target column not binary | Convert “Göç Etmemeyi Tercih Eder” into 0/1 first. |

---

##  License & Acknowledgment

- This project is for **research and educational purposes** only.  
- Respect data privacy and ethical use of survey data.  
- Special thanks to the field and data entry contributors.  
