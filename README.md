# 🎣 Phishing Detection Using Machine Learning

> Machine learning project that detects phishing websites by analyzing URL structure, hyperlink behavior, and page characteristics. Uses mutual-information feature selection with Logistic Regression and Random Forest to classify sites as legitimate or phishing.

![Python](https://img.shields.io/badge/python-3.x-blue)
![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-orange)
![Category](https://img.shields.io/badge/category-Cybersecurity-red)
![Accuracy](https://img.shields.io/badge/RF%20accuracy-97.7%25-brightgreen)

---

## 📖 About

Phishing websites imitate trusted sites to steal credentials and personal data. This project trains machine learning models to tell **legitimate** websites from **phishing** websites using 48 pre-extracted features, such as URL length, number of dots and dashes, IP address usage, external hyperlink percentage, and form behavior.

---

## 📊 Dataset

| Property | Value |
|----------|-------|
| File | `Phishing_Legitimate_full.csv` |
| Rows | 10,000 |
| Columns | 50 (`id`, 48 features, `CLASS_LABEL`) |
| Classes | 5,000 phishing (`1`) and 5,000 legitimate (`0`) |
| Balance | Perfectly balanced |

**Feature groups include:**
- **URL based:** `UrlLength`, `NumDots`, `NumDash`, `AtSymbol`, `IpAddress`, `NoHttps`, `PathLevel`
- **Hostname based:** `HostnameLength`, `SubdomainLevel`, `DomainInSubdomains`, `EmbeddedBrandName`
- **Page and form based:** `InsecureForms`, `ExtFormAction`, `SubmitInfoToEmail`, `IframeOrFrame`, `PopUpWindow`, `RightClickDisabled`
- **Hyperlink based:** `PctExtHyperlinks`, `PctExtResourceUrls`, `PctNullSelfRedirectHyperlinks`

---

## 🔬 Methodology

1. **Load and optimize data**: downcast `float64` to `float32` and `int64` to `int32` to save memory
2. **Exploratory analysis**: class distribution plot and correlation heatmaps across all feature groups
3. **Feature selection**: rank features with **Mutual Information** (`mutual_info_classif`)
4. **Model training**: train Logistic Regression and Random Forest on the top-N features (N from 20 to 50)
5. **Evaluation**: precision, recall, F1 score, accuracy, classification report, and confusion matrix

---

## 🤖 Models and Results

### Logistic Regression
Tested with the top 20 to 50 features. Accuracy stays in the range of roughly **92% to 93.5%** across feature counts.

### Random Forest (final model)

| Setting | Value |
|---------|-------|
| Features | Top 32 by mutual information |
| Trees | 500 |
| Max depth | 32 |
| Split | 80% train / 20% test, stratified, `random_state=42` |

| Metric | Score |
|--------|-------|
| Accuracy | **97.70%** |
| Precision | 97.70% |
| Recall | 97.70% |
| F1 Score | 97.70% |

Both classes score 0.98 precision and recall on 1,000 test samples each.

**Top predictive features:** `PctExtHyperlinks`, `PctExtResourceUrls`, `PctNullSelfRedirectHyperlinks`, `PctExtNullSelfRedirectHyperlinksRT`, `NumNumericChars`, `FrequentDomainNameMismatch`, `ExtMetaScriptLinkRT`, `NumDash`.

---

## ⚠️ Known Issue: Data Leakage in the Feature Sweep

The notebook's Random Forest loop (`train_rfc`, top 20 to 50 features) reports **100% accuracy**. This is **not a real result**. That function takes the first N columns of the dataset, which includes the `id` column. In this dataset, `id` perfectly separates the classes (phishing ids are 1 to 5000, legitimate ids are 5001 to 10000), so the model simply learns the row number.

When `id` is removed, accuracy drops to a realistic level. The **final 97.70% model** above is not affected, because it drops `id` before training.

**Recommended fix:** use `data.drop(['id', 'labels'], axis=1)` and select columns from the mutual-information ranking instead of `iloc[:, :top_n]`.

---

## 🧰 Tech Stack

- Python
- pandas, NumPy
- scikit-learn
- matplotlib, seaborn
- Google Colab / Jupyter Notebook

---

## 📁 Project Structure

```
Phishing-Detection-Using-Machine-Learning/
├── Phishing_Detection_Using_Machine_Learning.ipynb
├── Phishing_Legitimate_full.csv
└── README.md
```

---

## 🚀 Getting Started

1. **Clone the repository**
   ```bash
   git clone https://github.com/<your-username>/Phishing-Detection-Using-Machine-Learning.git
   cd Phishing-Detection-Using-Machine-Learning
   ```

2. **Install dependencies**
   ```bash
   pip install pandas numpy scikit-learn matplotlib seaborn jupyter
   ```

3. **Open the notebook**
   ```bash
   jupyter notebook Phishing_Detection_Using_Machine_Learning.ipynb
   ```

4. **Update the dataset path.** The notebook reads from `/content/Phishing_Legitimate_full.csv` (Google Colab). Change it to `Phishing_Legitimate_full.csv` when running locally.

5. Run all cells.

---

## 🔮 Future Improvements

- [ ] Fix the `id` leakage in the feature sweep
- [ ] Add cross-validation for more reliable scores
- [ ] Compare more models: XGBoost, SVM, Gradient Boosting
- [ ] Add hyperparameter tuning (GridSearchCV)
- [ ] Plot feature importance from the trained Random Forest
- [ ] Save the trained model with `joblib`
- [ ] Build a simple web app that predicts from a URL

---

## 📜 License

Released under the MIT License. Add a `LICENSE` file to your repo to enable this.

---

## 👤 Author

**Your Name**
- GitHub: [@your-github-username](https://github.com/your-github-username)

---

⭐ If you found this project useful, consider giving it a star!
