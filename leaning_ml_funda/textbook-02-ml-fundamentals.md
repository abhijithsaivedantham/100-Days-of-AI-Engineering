# Textbook 02 --- Practical Machine Learning Fundamentals

**Days 21--30 \| Goal:** Understand common model families and use
scikit-learn correctly, without heavy mathematical derivations.

## 1. ML workflow

A model learns patterns from examples. Supervised learning uses labels;
unsupervised learning does not. Features (`X`) are inputs; target (`y`)
is what a supervised model predicts.

Workflow: 1. Define the problem and success metric. 2. Inspect data and
target. 3. Split data before fitting preprocessing. 4. Establish a
baseline. 5. Train and compare suitable models. 6. Evaluate on
validation data. 7. Tune without touching the final test set. 8.
Evaluate once on held-out test data. 9. Save preprocessing and model
together. 10. Monitor performance and data drift.

## 2. Train/validation/test and leakage

Training fits model parameters. Validation compares approaches/tunes
settings. Test estimates performance on unseen data.

Leakage occurs when training uses information unavailable at prediction
time. Example: scaling all data before splitting, or using future values
to predict the past. Use scikit-learn `Pipeline` so preprocessing is
fitted within each training fold.

``` python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)
```

Use chronological splits for time-series. `stratify` is useful for many
classification tasks.

## 3. Model families

-   Linear regression: predicts numeric values; useful baseline for
    approximately linear relationships.
-   Logistic regression: classification probabilities; strong,
    interpretable baseline.
-   Decision tree: rule-like splits; interpretable but can overfit.
-   Random forest: combines trees; often robust for tabular data.
-   Gradient boosting: sequentially improves learners; strong for
    tabular data. XGBoost, LightGBM and CatBoost are popular libraries.
-   KNN: predicts from nearby examples; scaling matters and large
    inference sets can be costly.
-   SVM: margin-based decision boundary; scaling and kernel choice
    matter.
-   K-Means: partitions into a chosen number of clusters; sensitive to
    scaling and cluster shape.
-   DBSCAN: finds dense regions and labels sparse points as noise;
    parameters and varying densities matter.
-   Anomaly detection: finds unusual observations. Isolation Forest,
    LOF, One-Class SVM and autoencoders are examples. An anomaly is not
    automatically a fault.

For each model know: problem type, when to use it,
assumptions/limitations, input preparation, training API and evaluation.

## 4. Metrics

Classification: - Accuracy: fraction correct. - Precision: among
predicted positives, how many are truly positive? - Recall: among actual
positives, how many were found? - F1: balances precision and recall. -
Confusion matrix: true/false positives and negatives. - ROC-AUC: ranking
ability across thresholds; interpret with class balance and use case.

Regression: - MAE: average absolute error in target units. - MSE:
squared error, penalises large errors. - RMSE: square root of MSE, in
target units. - R²: relative fit measure; can be negative on test data.

Choose metrics based on error costs. In some applications missing a
positive is more costly than reviewing extra alerts.

## 5. Loss, optimizer and hyperparameters

A loss function measures prediction error that training attempts to
minimise. An optimizer updates model parameters. Hyperparameters are
choices such as tree depth or number of neighbours. An epoch is a pass
through training data; batch size is examples per update.

You need to know the purpose of a loss function and how libraries use
it---not derive every formula. In scikit-learn, `.fit()` handles
training for many estimators.

## 6. Pipeline example

``` python
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.impute import SimpleImputer
from sklearn.pipeline import Pipeline
from sklearn.linear_model import LogisticRegression

numeric = ["age", "spend"]
categorical = ["plan"]

prep = ColumnTransformer([
    ("num", Pipeline([
        ("imputer", SimpleImputer(strategy="median")),
        ("scale", StandardScaler()),
    ]), numeric),
    ("cat", Pipeline([
        ("imputer", SimpleImputer(strategy="most_frequent")),
        ("onehot", OneHotEncoder(handle_unknown="ignore")),
    ]), categorical),
])
model = Pipeline([
    ("prep", prep),
    ("classifier", LogisticRegression(max_iter=1000)),
])
model.fit(X_train, y_train)
```

## 7. Persistence

``` python
import joblib
joblib.dump(model, "model.joblib")
loaded = joblib.load("model.joblib")
```

Only load trusted pickle/joblib files; loading untrusted files can
execute code. Record library versions and assumptions.

### Mini-project A --- Churn classifier

Use public/synthetic data. Build a baseline, compare logistic regression
and random forest, report precision/recall/F1, inspect errors, save the
pipeline and write an inference script.

### Mini-project B --- Anomaly exploration

Compare Isolation Forest and DBSCAN on synthetic/public sensor data.
Visualise suspicious points and explain false positives. Unsupervised
scores are not ground truth.

## Assessment

Select a model family, create a leakage-safe pipeline, choose metrics,
explain overfitting, and serve predictions from a saved model.
