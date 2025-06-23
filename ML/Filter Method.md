## types
- Variance of threshold
- correlation
- anova 
- chi-squared
## variance threshold
```python
from sklearn.feature_selection import VarianceThreshold
selector = VarianceThreshold(threshold=<value>)
X_selected = selector.fit_transform(X)
%% usage %%
import numpy as np
from sklearn.feature_selection import VarianceThreshold

# Sample dataset
X = np.array([
    [0, 2, 0, 3],
    [0, 1, 4, 3],
    [0, 1, 1, 3]
])

# Initialize with default threshold = 0
selector = VarianceThreshold()  # removes features with 0 variance
X_selected = selector.fit_transform(X)

print("Original shape:", X.shape)
print("After variance threshold:", X_selected.shape)
```
#### 1. **Ignores the Target Variable**

- Variance Threshold is an **unsupervised** method.
    
- It selects features **without checking their correlation with the target**.
    
- Therefore, it may:
    
    - **Keep irrelevant features** that have high variance but no predictive value.
        
    - **Remove useful features** that have low variance but strong predictive power.
        

---

#### 2. **Ignores Feature Interactions**

- It evaluates **each feature independently**.
    
- A feature that appears uninformative on its own might be **very informative in combination** with another feature.
    
- This limits its effectiveness in identifying **important multi-feature patterns**.
    

---

#### 3. **Sensitive to Data Scaling**

- Features with large values naturally have **higher variance**.
    
- If the data is not standardized, features may be **wrongly retained or discarded** based on their scale rather than their informativeness.
    
- Solution: **Standardize or normalize** the data before applying this method.
    

```python
from sklearn.preprocessing import StandardScaler
X_scaled = StandardScaler().fit_transform(X)
```

---

#### 4. **Arbitrary Threshold Value**

- The choice of threshold is **manual** and may require **trial and error**.
    
- There's **no universal rule** to determine the best threshold.
    
- Low threshold (e.g., `0`) → removes only constant features.
    
- Higher threshold (e.g., `0.01`, `0.1`) → removes low-variance features, but risk of dropping useful ones.
### ✅ Correlation-Based Feature Selection (Filter Method)

Correlation-based feature selection involves removing features that are **highly correlated with each other** (to reduce multicollinearity) or **weakly correlated with the target** (to remove irrelevant features).

---

### 💡 Types of Correlation:

1. **Feature-to-Target Correlation**: Helps identify features that are **predictive** of the target.
    
2. **Feature-to-Feature Correlation**: Helps detect **redundant features** (those that are highly correlated with each other).
    

---

### 📌 Common Correlation Metrics:

- **Pearson correlation** (most common, linear relationship)
    
- **Spearman** (monotonic relationship)
    
- **Kendall Tau** (ordinal data)
    

---

### ✅ Feature Selection using Correlation (Manual Implementation using NumPy/Pandas)

#### 🔧 Step 1: Feature-to-Feature Correlation

```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

# Sample data
from sklearn.datasets import load_boston
boston = load_boston()
X = pd.DataFrame(boston.data, columns=boston.feature_names)

# Compute correlation matrix
corr_matrix = X.corr().abs()

# Plot the heatmap
plt.figure(figsize=(12, 8))
sns.heatmap(corr_matrix, annot=True, cmap='coolwarm')
plt.title("Feature Correlation Matrix")
plt.show()

# Remove highly correlated features (e.g., threshold > 0.9)
upper = corr_matrix.where(np.triu(np.ones(corr_matrix.shape), k=1).astype(bool))

to_drop = [column for column in upper.columns if any(upper[column] > 0.9)]
X_filtered = X.drop(columns=to_drop)

print("Dropped features due to high correlation:", to_drop)
```

---

#### 🔧 Step 2: Feature-to-Target Correlation

```python
# Add target column
y = boston.target
X_with_target = X.copy()
X_with_target['TARGET'] = y

# Compute correlation with target
correlation_with_target = X_with_target.corr()['TARGET'].abs().sort_values(ascending=False)

# Select top correlated features
top_features = correlation_with_target[correlation_with_target > 0.3].index.tolist()
top_features.remove('TARGET')  # remove target itself

print("Selected Features based on correlation with target:\n", top_features)
```

---

### ❓ Can we do this in `sklearn`?

No direct built-in function in `sklearn` selects features using correlation. But you can:

- Use **`SelectKBest(score_func=f_regression)`** which internally uses correlation-like logic for regression tasks.
    
- Or use **custom functions** as shown above using `pandas`.
### ✅ ANOVA (Analysis of Variance) for Feature Selection

The **ANOVA F-test** is a statistical test used in feature selection to determine the **linear dependency between each feature and the target**. It's commonly used for **classification tasks**, especially when the **target is categorical** and the **features are numerical**.

---

### 📘 What It Does:

- Measures **variance between groups** (i.e., target classes) relative to **variance within groups**.
    
- A **higher F-value** means the feature is **more discriminative** for the target classes.
    

---

### ✅ Use in Scikit-Learn: `SelectKBest` with `f_classif`

```python
from sklearn.feature_selection import SelectKBest, f_classif
from sklearn.datasets import load_iris
import pandas as pd

# Load dataset
data = load_iris()
X = pd.DataFrame(data.data, columns=data.feature_names)
y = data.target

# Select top 2 features based on ANOVA F-test
selector = SelectKBest(score_func=f_classif, k=2)
X_selected = selector.fit_transform(X, y)

# Show selected features
selected_features = X.columns[selector.get_support()]
print("Selected features:\n", selected_features)
```

---

### 🧠 When to Use ANOVA?

|Criteria|Recommendation|
|---|---|
|Target type|Categorical (e.g., classes: 0,1,2)|
|Feature type|Numerical|
|Goal|Rank/select features for classification|
|Assumption|Data should be approximately normal|

---

### 📊 Visualizing ANOVA Scores

```python
import matplotlib.pyplot as plt

f_values, _ = f_classif(X, y)
plt.bar(x=X.columns, height=f_values)
plt.xticks(rotation=45)
plt.title("ANOVA F-values for each feature")
plt.ylabel("F-score")
plt.tight_layout()
plt.show()
```
### ✅ Chi-Squared (χ²) Test for Feature Selection

The **Chi-Squared test** is a **filter method** used to select features that are **independent** of the target variable. It works by measuring the **statistical dependence between each feature and the target**, assuming:

- Features are **non-negative and categorical/discrete**
    
- Target is **categorical** (used mostly in classification problems)
    

---

### 📘 When to Use Chi-Squared?

|Criteria|Requirement|
|---|---|
|Target variable|Categorical|
|Features|Categorical or non-negative numerical|
|Goal|Rank/select features for classification|

> ❗ Continuous numerical features must be **discretized or binned** before using Chi-Squared.

---

### ✅ Chi-Squared in Scikit-learn

```python
from sklearn.datasets import load_iris
from sklearn.feature_selection import SelectKBest, chi2
from sklearn.preprocessing import MinMaxScaler
import pandas as pd

# Load Iris data
data = load_iris()
X = pd.DataFrame(data.data, columns=data.feature_names)
y = data.target

# Step 1: Chi² requires non-negative values → use MinMaxScaler
scaler = MinMaxScaler()
X_scaled = scaler.fit_transform(X)

# Step 2: Select top 2 features using Chi²
selector = SelectKBest(score_func=chi2, k=2)
X_selected = selector.fit_transform(X_scaled, y)

# Print selected feature names
selected_features = X.columns[selector.get_support()]
print("Selected Features using Chi²:\n", selected_features)
```

---

### 📊 Visualizing Chi-Squared Scores

```python
import matplotlib.pyplot as plt

chi_scores, _ = chi2(X_scaled, y)
plt.bar(x=X.columns, height=chi_scores)
plt.xticks(rotation=45)
plt.title("Chi-Squared Scores for Features")
plt.ylabel("Chi² Score")
plt.tight_layout()
plt.show()
```

---

### 🧠 How it Works:

- Tests **H0 (null hypothesis)**: Feature and target are independent.
    
- Large Chi² value = **higher dependency** = **more relevant**.

## ✅ Advantages of Filter Methods

---

### 1. **Simplicity**

- Easy to understand and implement.
    
- Use straightforward statistical tests (e.g., correlation, Chi-square, ANOVA).
    
- Do not require model training.
    

---

### 2. **Speed**

- Very fast and computationally efficient.
    
- Evaluate each feature **independently** of others.
    
- Much quicker than wrapper or embedded methods.
    

---

### 3. **Scalability**

- Can easily handle **high-dimensional datasets** (e.g., text or gene data).
    
- Suitable for problems with **hundreds or thousands of features**.
    

---

### 4. **Good as a Preprocessing Step**

- Can be used to reduce dimensionality **before** applying expensive wrapper or embedded methods.
    
- Helps eliminate clearly irrelevant features early in the pipeline.


## ❌ Disadvantages of Filter Methods

---

### 1. **Lack of Feature Interaction**

- Treat each feature in isolation.
    
- Ignore **interactions or synergies** between features.
    
- May discard features that are only useful **in combination**.
    

---

### 2. **Model-Agnostic**

- Not tailored to any specific machine learning model.
    
- Features selected might not be optimal for the final model’s performance.
    

---

### 3. **Statistical Measure Limitations**

- Each filter relies on specific statistical assumptions:
    
    - **Correlation** assumes a **linear relationship**.
        
    - **Chi-square** assumes **independence and discrete data**.
        
    - **Variance** does not measure relevance to target.
        
- These may not capture **complex, non-linear relationships**.
    

---

### 4. **Threshold Determination**

- Requires defining arbitrary cutoffs (e.g., variance > 0.01, correlation < 0.8).
    
- These thresholds can vary between datasets and may need tuning.
