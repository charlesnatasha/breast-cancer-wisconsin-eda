*# Breast Cancer Wisconsin: EDA \& Baseline Classifier*



*Exploratory data analysis and a baseline logistic regression classifier on the Breast Cancer Wisconsin Diagnostic dataset (569 samples, 30 numeric features).*



*!\[Correlation Heatmap](images/Correlation\_heatmap.png)*



*## Overview*



*Identify which cell-nuclei measurements best distinguish malignant from benign tumors, and build a transparent baseline classifier that a non-ML audience could trust.*



*## Dataset*



*- \*\*Source:\*\* \[Kaggle - Breast Cancer Wisconsin Diagnostic](https://www.kaggle.com/datasets/uciml/breast-cancer-wisconsin-data)*

*- \*\*Samples:\*\* 569 (357 Benign, 212 Malignant)*

*- \*\*Features:\*\* 30 numeric (mean, standard error, and "worst" versions of 10 cell-nuclei measurements)*

*- \*\*Target:\*\* `diagnosis` (0 = Benign, 1 = Malignant)*



*## Data Cleaning*



*- No missing values, no duplicates*

*- Stripped `x.` prefix from feature names for readability*

*- Renamed `y` to `diagnosis` and created a `diagnosis\_label` column for plots*

*- No rows dropped - the dataset arrived pre-cleaned*



*## Key Findings*



*### Univariate*

*- Most features are right-skewed, suggesting standardization benefits distance-based models.*

*- Size-related features (`radius\_\*`, `perimeter\_\*`, `area\_\*`) show clear bimodal separation between classes - the distributions form distinct "candles" for Benign vs. Malignant.*

*- `fractal\_dim\_\*` and `smoothness\_\*` show heavy overlap - weak single-feature predictors.*



*### Bivariate*

*- Ranking by percent mean difference: `concavity\_mean` (+249%), `area\_se` (+244%), `concave\_pts\_mean` (+242%) top the list.*

*- The `\_worst` (largest cell nuclei) features show the cleanest class separation - malignant tumors have more extreme outliers.*



*### Correlation*

*- Three feature families emerged:*

&#x20; *- Size (9 features, |r| > 0.94): radius / perimeter / area*

&#x20; *- Shape (6 features, |r| > 0.85): concavity / concave\_pts / compactness*

&#x20; *- Texture (weakly correlated): smoothness / symmetry / fractal\_dim*

*- Severe multicollinearity: `radius\_mean` and `perimeter\_mean` correlate at 0.998. Handled via L2 regularization in the logistic regression baseline.*



*### Coefficient Interpretation*

*- The largest negative coefficient is `compactness\_se` - a classic multicollinearity artifact: because `compactness\_se` correlates with other shape features, the model redistributes their joint signal and assigns one a "corrective" negative weight. L2 regularization limits how large these opposing coefficients become, keeping the model stable.*



*## Model*



*\*\*Logistic Regression\*\* with L2 (ridge) regularization and feature standardization (train/test split 80/20, stratified).*



*| Metric | Value |*

*|---|---|*

*| Test Accuracy | 96.49% |*

*| ROC-AUC | 0.9960 |*

*| Precision (Malignant) | 0.97 |*

*| Recall (Malignant) | 0.93 |*

*| Precision (Benign) | 0.96 |*

*| Recall (Benign) | 0.99 |*



*\*\*Confusion matrix (test set):\*\**



*| | Predicted Benign | Predicted Malignant |*

*|---|---|---|*

*| \*\*Actual Benign\*\* | 71 | 1 |*

*| \*\*Actual Malignant\*\* | 3 | 39 |*



*\*\*Clinical note:\*\* Recall for Malignant is 0.93 - the model misses \~7% of cancers (3 false negatives). In a clinical context, false negatives are far costlier than false positives, so a production model would tune the decision threshold downward (e.g., 0.3) to push malignant recall toward 0.98+, accepting more false positives as the trade-off.*



*!\[Model Evaluation](images/model\_evaluation.png)*



*## Tech Stack*



*Python, Pandas, NumPy, Matplotlib, Seaborn, Scikit-learn*



*## Project Structure*



*```*

*breast-cancer-wisconsin-eda/*

*|-- notebooks/*

*|   `-- BreastcancerEDA.ipynb*

*|-- data/*

*|   `-- brca.csv*

*|-- images/*

*|   |-- univariate\_grid.png*

*|   |-- univariate\_worst.png*

*|   |-- boxplots\_all.png*

*|   |-- top6\_features.png*

*|   |-- Correlation\_heatmap.png*

*|   |-- clustermap.png*

*|   |-- target\_correlation.png*

*|   |-- model\_evaluation.png*

*|   `-- coefficients.png*

*|-- .gitignore*

*`-- README.md*

*```*



*## How to Run*



*```bash*

*git clone https://github.com/charlesnatasha/breast-cancer-wisconsin-eda.git*

*cd breast-cancer-wisconsin-eda*

*pip install pandas numpy matplotlib seaborn scikit-learn jupyter*

*jupyter notebook notebooks/BreastcancerEDA.ipynb*

*```*



*## Next Steps*



*- Compare with tree-based models (Random Forest, XGBoost)*

*- Feature selection with SelectKBest or RFE*

*- Compare a reduced feature set (size + shape only) against the full 30-feature model*

*- Cross-validation for more robust performance estimates*

*- Threshold tuning to optimize malignant recall for a clinical setting*



*## Author*



*\*\*Natasha Charles\*\**

*- GitHub: \[@charlesnatasha](https://github.com/charlesnatasha)*

