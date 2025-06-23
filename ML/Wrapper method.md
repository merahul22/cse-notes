
## 🧠 Wrapper Methods for Feature Selection

Wrapper methods are a category of feature selection techniques that evaluate subsets of features based on the predictive power of a machine learning model. Unlike filter methods that assess features independently of any model, wrapper methods _wrap_ a model around the feature selection process.

---

### 🔄 Working Mechanism

1. ### **Subset Generation**
    
    Generate a candidate subset of features using one of the following strategies:
    
    - **Forward Selection**: Start with no features and add one at a time.
        
    - **Backward Elimination**: Start with all features and remove one at a time.
        
    - **Exhaustive Search**: Try all possible combinations (computationally expensive).
        
    - **Random Subsets**: Try random feature combinations.
        
2. ### **Subset Evaluation**
    
    - Train a predictive model (e.g., Linear Regression, Decision Tree) on the selected subset.
        
    - Evaluate the model performance using **cross-validation**.
        
    - The resulting performance score is used to assess the "quality" of the feature subset.
        
3. ### **Stopping Criterion**
    
    - Stop when:
        
        - Maximum number of iterations is reached.
            
        - No further improvement in model accuracy is observed.
            
        - A performance threshold is met.
            

---

### 📊 Common Algorithms

| Algorithm                               | Strategy                                       | Description                                                             |
| --------------------------------------- | ---------------------------------------------- | ----------------------------------------------------------------------- |
| **Forward Selection**                   | Add one feature at a time                      | Start from an empty set and add features that improve model performance |
| **Backward Elimination**                | Remove one feature at a time                   | Start with all features and remove the least useful                     |
| **Recursive Feature Elimination (RFE)** | Recursively eliminate least important features | Uses model importance (e.g., coefficients or feature importances)       |

---

### ✅ Advantages

- Takes feature **interactions** into account.
    
- Often results in **high-performing** feature subsets.
    
- More **accurate** than filter methods (though slower).
    

---

### ❌ Disadvantages

- **Computationally expensive**, especially with large feature sets.
    
- Prone to **overfitting** if cross-validation is not handled properly.
    
- Heavily dependent on the choice of **model** used for evaluation.


### ✅ Objective

We'll use **Exhaustive Feature Selection (EFS)** with a **Logistic Regression** model on the **Breast Cancer** dataset from `sklearn.datasets`.

---

### 📦 Required Libraries

```bash
pip install mlxtend scikit-learn
```

---

### 🧪 Full Python Code

```python
from sklearn.datasets import load_breast_cancer
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from mlxtend.feature_selection import ExhaustiveFeatureSelector as EFS
from sklearn.metrics import accuracy_score
import pandas as pd

# Load dataset
data = load_breast_cancer()
X, y = data.data, data.target
feature_names = data.feature_names

# Split data into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Initialize Logistic Regression
lr = LogisticRegression(max_iter=10000)

# Exhaustive Feature Selector (max 4 features, 5-fold CV, accuracy scoring)
efs = EFS(estimator=lr,
          min_features=1,
          max_features=4,
          scoring='accuracy',
          print_progress=True,
          cv=5)

# Fit the EFS
efs = efs.fit(X_train, y_train)

# Best feature subset
best_features_idx = list(efs.best_idx_)
print("Best features (indices):", best_features_idx)
print("Best features (names):", [feature_names[i] for i in best_features_idx])

# Evaluate model using selected features
lr.fit(X_train[:, best_features_idx], y_train)
y_pred = lr.predict(X_test[:, best_features_idx])

# Accuracy
print("Test Accuracy with Best Subset:", accuracy_score(y_test, y_pred))
```

---

### 💡 Output (Example)

You’ll see:

- Progress for each combination being evaluated.
    
- Indices and names of the selected best subset of features.
    
- Accuracy score on the test set using only the selected features.
    

---

### ⚠️ Notes

- **Computational Cost**: For large datasets, restrict `max_features` (e.g., to 3–5) to avoid long runtimes.
    
- **Model-Specific**: EFS performance varies depending on the estimator used (`LogisticRegression`, `SVC`, etc.).
    
- **Cross-Validation**: Ensure CV is sufficient to prevent overfitting but not too large for small datasets



## 🧠 What is Sequential Backward Selection (SBS)?

**SBS** is a **wrapper-based** feature selection method. Unlike forward selection (which starts with no features and adds them), **SBS starts with all features** and progressively **removes the least significant feature** at each step to improve model performance.

---

### 🔄 How It Works

1. **Start** with the full set of features.
    
2. At each iteration:
    
    - **Remove one feature** whose removal **minimally reduces** or improves model performance.
        
3. Evaluate performance using **cross-validation** and a **scoring metric** (e.g., accuracy).
    
4. Continue until the desired number of features is reached.
    

---

### ✅ Advantages

- Less computationally expensive than exhaustive search.
    
- Can capture feature interactions.
    

### ❌ Disadvantages

- Still slower than filter methods.
    
- May get stuck in local optima (greedy approach).
    

---

## 🧪 Code: Sequential Backward Selection (SBS) using `mlxtend`

```python
from sklearn.datasets import load_breast_cancer
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from mlxtend.feature_selection import SequentialFeatureSelector as SFS
from sklearn.metrics import accuracy_score
import pandas as pd

# Load the dataset
data = load_breast_cancer()
X, y = data.data, data.target
feature_names = data.feature_names

# Split the dataset
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Create model
lr = LogisticRegression(max_iter=10000)

# Apply Sequential Backward Selection
sbs = SFS(estimator=lr,
          k_features=4,
          forward=False,   # Important: backward selection
          floating=False,
          scoring='accuracy',
          cv=5,
          n_jobs=-1,
          print_progress=True)

# Fit the model
sbs = sbs.fit(X_train, y_train)

# Get selected feature indices and names
selected_idx = list(sbs.k_feature_idx_)
selected_features = [feature_names[i] for i in selected_idx]
print("Selected Feature Indices:", selected_idx)
print("Selected Feature Names:", selected_features)

# Train model on selected features
lr.fit(X_train[:, selected_idx], y_train)

# Predict and evaluate
y_pred = lr.predict(X_test[:, selected_idx])
print("Test Accuracy with SBS-selected features:", accuracy_score(y_test, y_pred))
```

---

### 📊 Sample Output

```text
[Progress bar shows selection]
Selected Feature Indices: [0, 4, 20, 27]
Selected Feature Names: ['mean radius', 'mean smoothness', 'worst area', 'worst smoothness']
Test Accuracy: 0.9561
```

---

### 📘 Summary

- `forward=False` → Backward selection.
    
- `k_features=4` → Final number of selected features.
    
- `floating=False` → Disables SFFS (floating variants).


## 🧠 What is Forward Feature Selection (SFS)?

**Sequential Forward Selection (SFS)** is a **wrapper-based** method that starts with **no features** and **adds one feature at a time** that **improves the model performance the most** until the desired number of features is selected.

---

### 🔄 How SFS Works

1. **Start** with an empty feature set.
    
2. **Iteratively add** the feature that improves the model performance the most (e.g., accuracy via CV).
    
3. Continue until:
    
    - Desired number of features (`k_features`) is reached.
        
    - Or performance plateaus (optional early stopping).
        

---

### ✅ Advantages

- Faster than exhaustive search.
    
- Captures feature interaction progressively.
    
- Simple and effective.
    

### ❌ Disadvantages

- Greedy: may not find the global optimum.
    
- Computationally more expensive than filter methods.
    

---

## 🧪 Minimal Code Changes for Forward Selection

In your SBS code above, **only one change is needed**:

```python
forward=False  → forward=True
```

---

## ✅ Full Code for Forward Feature Selection (SFS)

```python
from sklearn.datasets import load_breast_cancer
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from mlxtend.feature_selection import SequentialFeatureSelector as SFS
from sklearn.metrics import accuracy_score

# Load data
data = load_breast_cancer()
X, y = data.data, data.target
feature_names = data.feature_names

# Train-test split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Logistic Regression model
lr = LogisticRegression(max_iter=10000)

# Sequential Forward Selection
sfs = SFS(estimator=lr,
          k_features=4,
          forward=True,        # 👈 Forward selection enabled
          floating=False,
          scoring='accuracy',
          cv=5,
          n_jobs=-1,
          print_progress=True)

# Fit the selector
sfs = sfs.fit(X_train, y_train)

# Get selected feature indices and names
selected_idx = list(sfs.k_feature_idx_)
selected_features = [feature_names[i] for i in selected_idx]
print("Selected Feature Indices:", selected_idx)
print("Selected Feature Names:", selected_features)

# Train and evaluate on selected features
lr.fit(X_train[:, selected_idx], y_train)
y_pred = lr.predict(X_test[:, selected_idx])
print("Test Accuracy with SFS-selected features:", accuracy_score(y_test, y_pred))
```

---

### 📘 Summary of What You Changed:

|Parameter|SBS Value|SFS Value|
|---|---|---|
|`forward`|`False`|`True`|
|`floating`|`False`|`False`|


## 🧠 **Recursive Feature Elimination (RFE)** — Theory

**RFE** is a **wrapper-based** feature selection technique. It works by **recursively removing the least important features**, based on a model’s importance scores, until a desired number of features is reached.

### 🔄 **How RFE Works:**

1. **Train a model** (e.g., RandomForest, SVM, LogisticRegression) on the full set of features.
    
2. **Rank the features** by their importance (e.g., coefficients, Gini importance, etc.).
    
3. **Remove the least important feature**.
    
4. **Repeat** the process on the remaining features.
    
5. **Stop** when the desired number of features (`n_features_to_select`) is reached.
    

---

### ✅ **Advantages**

- Considers feature interaction.
    
- Works well with any estimator that exposes feature importance or coefficients.
    
- More accurate than filter methods.
    

---

### ❌ **Disadvantages**

- **Computationally expensive**, especially with many features.
    
- **Model-dependent**: RFE results vary with the chosen model.
    
- May overfit if cross-validation is not used properly.
    

---

### 📦 Example Using `RandomForestClassifier`

```python
from sklearn.feature_selection import RFE
from sklearn.ensemble import RandomForestClassifier
```

Why Random Forest?

- It provides `feature_importances_` which RFE uses to rank features.
    

```python
rfe = RFE(estimator=model, n_features_to_select=1)
```

What does this do?

- It **keeps eliminating** the least important features until only **1 feature** remains.
    
- It records the **ranking of each feature** as it was eliminated.
    

---

### 🧪 Output Interpretation

If `petal_length` has rank 1, it means it was the **last feature eliminated**, i.e., the **most important**.

---

### 📘 Summary Table

|Step|Description|
|---|---|
|1|Train model on current features|
|2|Rank features by importance|
|3|Remove least important feature|
|4|Repeat until desired number of features remains|
```python
import pandas as pd
from sklearn.ensemble import RandomForestClassifier
from sklearn.feature_selection import RFE
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelEncoder

# Load Iris dataset
url = "https://gist.githubusercontent.com/curran/a08a1080b88344b0c8a7/raw/0e7a9b0a5d22642a06d3d5b9bcbad9890c8ee534/iris.csv"
df = pd.read_csv(url)

# Separate features and target
X = df.drop("species", axis=1)
y = df["species"]

# Encode the target labels
le = LabelEncoder()
y_encoded = le.fit_transform(y)

# Split data (optional, but useful for training/testing later)
X_train, X_test, y_train, y_test = train_test_split(X, y_encoded, test_size=0.2, random_state=42)

# Initialize RandomForestClassifier
model = RandomForestClassifier(n_estimators=100, random_state=42)

# RFE: Select 1 best feature (ranking all features)
rfe = RFE(estimator=model, n_features_to_select=1)
rfe.fit(X_train, y_train)

# Feature ranking
print("Feature Ranking (1 = most important):\n")
for feature, rank in zip(X.columns, rfe.ranking_):
    print(f"{feature}: {rank}")

```

