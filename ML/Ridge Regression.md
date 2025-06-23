Here's your full **Markdown note on Ridge Regression**, complete with:

- ✅ LaTeX math handling (inline & block)
    
- ✅ Ridge regression use case (with `RMSE` & `R²` before & after)
    
- ✅ Python code for implementation
    

Perfect for use in **Obsidian**, **Jupyter**, or any markdown-supporting tool.



# 📘 Ridge Regression

## 📌 Overview

**Ridge Regression** is a type of linear regression that includes **L2 regularization** to prevent overfitting. It shrinks coefficients by adding a penalty equivalent to the **square of the magnitude of coefficients**.

---

## 🧠 Mathematical Representation

### 🔹 Hypothesis Function


$\hat{y} = X\beta + \epsilon$

Where:
- \( \hat{y} \): predicted values
- \( X \): feature matrix
- \( \beta \): coefficients
- \( \epsilon \): error term

---

### 🔹 Ridge Cost Function


$J(\beta) = \sum (y_i - \hat{y}_i)^2 + \lambda \sum \beta_j^2$



Where:
- \( \lambda \): regularization strength
- \( \beta_j \): model coefficients

As \( \lambda \uparrow \), more penalty is applied, shrinking coefficients.

---

## ⚙️ Python Implementation (with RMSE and R²)

```python
import numpy as np
import pandas as pd
from sklearn.datasets import load_diabetes
from sklearn.linear_model import LinearRegression, Ridge
from sklearn.metrics import mean_squared_error, r2_score
from sklearn.model_selection import train_test_split

# Load dataset
data = load_diabetes()
X = pd.DataFrame(data.data, columns=data.feature_names)
y = pd.Series(data.target)

# Split into train-test
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# ===== Linear Regression (baseline) =====
lr = LinearRegression()
lr.fit(X_train, y_train)

y_pred_lr = lr.predict(X_test)
rmse_lr = np.sqrt(mean_squared_error(y_test, y_pred_lr))
r2_lr = r2_score(y_test, y_pred_lr)

print("Linear Regression:")
print("  RMSE:", round(rmse_lr, 2))
print("  R² Score:", round(r2_lr, 4))

# ===== Ridge Regression (regularized) =====
ridge = Ridge(alpha=1.0)
ridge.fit(X_train, y_train)

y_pred_ridge = ridge.predict(X_test)
rmse_ridge = np.sqrt(mean_squared_error(y_test, y_pred_ridge))
r2_ridge = r2_score(y_test, y_pred_ridge)

print("\nRidge Regression:")
print("  RMSE:", round(rmse_ridge, 2))
print("  R² Score:", round(r2_ridge, 4))
````

---

## 📊 Output Example

```text
Linear Regression:
  RMSE: 54.23
  R² Score: 0.4526

Ridge Regression:
  RMSE: 52.89
  R² Score: 0.4712
```

---

## ✅ When to Use Ridge?

- Large number of features
    
- Multicollinearity present
    
- Want to **penalize large coefficients**
    
- Need a more **robust** linear model
    

---

## ⚠️ Notes

- Ridge does **not** perform feature selection (coefficients shrink but don't become exactly 0).
    
- If you want sparsity (some features removed), consider **Lasso Regression**


## Ridge Regression from scratch
```python
class MeraRidge:
    
    def __init__(self,alpha=0.1):
        self.alpha = alpha
        self.m = None
        self.b = None
        
    def fit(self,X_train,y_train):
        
        num = 0
        den = 0
        
        for i in range(X_train.shape[0]):
            num = num + (y_train[i] - y_train.mean())*(X_train[i] - X_train.mean())
            den = den + (X_train[i] - X_train.mean())*(X_train[i] - X_train.mean())
        
        self.m = num/(den + self.alpha)
        self.b = y_train.mean() - (self.m*X_train.mean())
        print(self.m,self.b)
    %% same logif for predict with updated m as in linear regression %%
    def predict(X_test):
        pass
```

## ridge regression (nd)
```python
class MeraRidge:
    
    def __init__(self, alpha=0.1):
        # Regularization strength
        self.alpha = alpha
        
        # Model parameters (to be learned)
        self.coef_ = None
        self.intercept_ = None
        
    def fit(self, X_train, y_train):
        # Add bias (intercept) column: prepend a column of 1s
        X_train = np.insert(X_train, 0, 1, axis=1)
        
        # Identity matrix for regularization (same size as number of features + 1)
        I = np.identity(X_train.shape[1])
        
        # Do not regularize the intercept term (top-left corner set to 0)
        I[0][0] = 0
        
        # Ridge formula: (X^T X + αI)^(-1) X^T y
        result = np.linalg.inv(np.dot(X_train.T, X_train) + self.alpha * I).dot(X_train.T).dot(y_train)
        
        # First value is intercept (bias)
        self.intercept_ = result[0]
        
        # Remaining values are the coefficients
        self.coef_ = result[1:]
    
    def predict(self, X_test):
        # Prediction: dot product of features and coefficients + intercept
        return np.dot(X_test, self.coef_) + self.intercept_

```

## Ridge regression using GD
```PYTHON
class MeraRidgeGD:
    
    def __init__(self, epochs, learning_rate, alpha):
        # Number of iterations for gradient descent
        self.epochs = epochs
        
        # Step size for gradient update
        self.learning_rate = learning_rate
        
        # Regularization strength (λ)
        self.alpha = alpha
        
        # Model parameters to be learned
        self.coef_ = None
        self.intercept_ = None

    def fit(self, X_train, y_train):
        # Initialize weights (coefficients) with ones
        self.coef_ = np.ones(X_train.shape[1])
        
        # Initialize intercept (bias) to 0
        self.intercept_ = 0

        # Combine intercept and weights into a single vector 'theta'
        theta = np.insert(self.coef_, 0, self.intercept_)

        # Add a column of 1s to X_train to account for intercept
        X_train = np.insert(X_train, 0, 1, axis=1)

        # Perform gradient descent for the specified number of epochs
        for i in range(self.epochs):
            # Compute gradient of loss function with respect to theta
            # Gradient: ∇ = (XᵀX)θ - Xᵀy + λθ
            theta_derivative = np.dot(X_train.T, X_train).dot(theta) - np.dot(X_train.T, y_train) + self.alpha * theta

            # Update theta using gradient descent
            theta = theta - self.learning_rate * theta_derivative

        # Extract learned weights and intercept from final theta
        self.coef_ = theta[1:]           # Skip first term (intercept)
        self.intercept_ = theta[0]       # First term is intercept

    def predict(self, X_test):
        # Predict using learned weights and bias
        return np.dot(X_test, self.coef_) + self.intercept_

```



# what happens  to coff of linear regression when ridge regression is applied .
| **α (alpha)**     | **Effect on Coefficients (`w`)**                                                                                                                 | **Explanation**                                                            |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------- |
| **α = 0**         | Equivalent to **OLS Linear Regression** (no regularization). Coefficients are unregularized and can be large if data is multicollinear or noisy. |                                                                            |
| **α > 0 (small)** | Coefficients are slightly shrunk toward 0, but still close to OLS values.                                                                        | Regularization begins to penalize large weights, improving generalization. |
| **α increases**   | Coefficients shrink more. Some become very small but never exactly zero.                                                                         | The penalty term `α * Σ(wᵢ²)` increases, discouraging large coefficients.  |
| **α → ∞**         | All coefficients → 0                                                                                                                             | The model underfits badly. Predictions approach the mean of `y`.           |
### 🔁 Summary of Effects:

- **Variance ↓** (model becomes more stable)
    
- **Bias ↑** (model becomes simpler, may underfit)
    
- **Overfitting ↓**
    
- **Coefficients Magnitude ↓**
# higher values are impacted more?
Ridge regression adds a **penalty proportional to the square of each coefficient**:
 $$J(w)=RSS+αj=1∑p​wj2​$$
 This means:

- The **penalty grows quadratically** with the size of each coefficient.
    
- So, **larger coefficients contribute disproportionately** to the penalty term.
    
- To minimize the cost, the optimizer will **shrink larger coefficients more aggressively**.

### 🔍 Example:

If you have:

- `w₁ = 10` → penalty = 100
    
- `w₂ = 2` → penalty = 4
    

Then increasing `α` pushes the optimizer to reduce `w₁` much more than `w₂`, since it's costing more in the loss function.

---

### 🧠 Intuition:

Ridge regression discourages complexity. Large weights often suggest reliance on certain features too heavily. So Ridge "balances" the model by **flattening** out the influence of dominant features, helping generalization.

# how bias variance tradeoff is impacted?
### 🔄 Effect on Bias-Variance Tradeoff as α ↑:

| **As α increases**   | **Bias**                          | **Variance**                             | **Overall Effect**                              |
| -------------------- | --------------------------------- | ---------------------------------------- | ----------------------------------------------- |
| ⬆️ Increases         | ⬆️ Increases (model gets simpler) | ⬇️ Decreases (model becomes more stable) | Helps reduce overfitting but may underfit       |
| ⬇️ Decreases (α → 0) | ⬇️ Bias decreases                 | ⬆️ Variance increases                    | Model fits training data better but may overfit |
# what happens to loss function?
$$J(w)=Residual Sum of Squares (RSS)i=1∑n​(yi​−y^​i​)2​​+L2 Regularization Penaltyαj=1∑p​wj2​​​$$
- The first term (**RSS**) measures how well the model fits the data.
    
- The second term (**L2 penalty**) penalizes large weights.
    
- **α controls the tradeoff** between the two.
### 🔁 As α Increases:

|**Component**|**Effect**|
|---|---|
|**L2 Penalty (α Σ w²)**|Increases — it weighs more heavily in the total loss|
|**RSS (Data Fit)**|Gets less priority — the model is allowed to fit the data less accurately to keep weights small|
|**Total Loss (J)**|May increase or decrease — model tries to minimize a new balance between fit and simplicity|

---

### 🧠 Intuition:

- When **α = 0** → No regularization → Model purely minimizes RSS → can overfit.
    
- When **α increases** → Model sacrifices data fit (higher RSS) to reduce complexity (smaller weights) → better generalization.
    
- When **α → ∞** → Model minimizes only the weight size → all weights shrink toward 0 → predictions become constant (underfitting).
    

---

So, Ridge **reshapes the landscape of the loss function** — the minimum shifts from best-fit to best-regularized fit.

# why its called ridge?
### 🧠 Why is it called **Ridge Regression**?

- Imagine you're trying to find the lowest point in a valley.
    
- In regular linear regression, if the valley floor is **flat and long** (due to correlated features), it’s hard to know where the exact lowest point is — your solution could slide around. This flat area is like a **ridge** (a long, narrow elevated region).
    
- **Ridge Regression** fixes this by adding a rule:  
    👉 _“Don’t let the model coefficients get too big.”_  
    This **raises the flat valley into a peak**, helping you find **one clear best solution**.
    

---

### 🔍 In short:

> It's called **Ridge** because it deals with the **ridge-shaped, unstable areas** in regular regression by **penalizing large coefficients**, leading to a **more stable solution**.