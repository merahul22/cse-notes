

# Linear Regression

## Overview

Linear Regression is a supervised learning algorithm used to model the relationship between a scalar dependent variable (y) and one or more independent variables (X). It assumes a linear relationship:

y=β0​+β1​x1​+β2​x2​+⋯+βn​xn​+ε

- **Simple Linear Regression:** single feature (n=1).
- **Multiple Linear Regression:** multiple features (n>1).

---

## Assumptions

- **Linearity:** Relationship between X and y is linear.
- **Independence:** Observations are independent.
- **Homoscedasticity:** Errors have constant variance.
- **Normality:** Errors are normally distributed.
- **No multicollinearity:** Features are not highly correlated.

---

## Key Concepts & Formulas

### Cost Function (MSE)

J(β)=2m1​∑i=1m​(y^​(i)−y(i))2

### Ordinary Least Squares (Closed-form)

β^​=(X⊤X)−1X⊤y

### Gradient Descent

**Update rule:**

βj​:=βj​−α∂βj​∂​J(β)

**Gradient:**

∂βj​∂​J=m1​∑i=1m​(y^​(i)−y(i))xj(i)​

---

## Performance Metrics

- Mean Squared Error (MSE)
- Root Mean Squared Error (RMSE)
- Coefficient of Determination (R2)

---

## Implementation From Scratch (Python)

Python

```python
class MeraLR:
    
    def __init__(self):
        self.m = None  # Slope
        self.b = None  # Intercept
        
    def fit(self, X_train, y_train):
        X_mean = X_train.mean()
        y_mean = y_train.mean()

        num = 0
        den = 0

        for i in range(X_train.shape[0]):
            num += (X_train[i] - X_mean) * (y_train[i] - y_mean)
            den += (X_train[i] - X_mean) ** 2

        self.m = num / den
        self.b = y_mean - (self.m * X_mean)

        print(f"Fitted slope (m): {self.m}")
        print(f"Fitted intercept (b): {self.b}")
    
    def predict(self, X_test):
        return self.m * X_test + self.b

%% usage %%
import numpy as np

# Training data
X_train = np.array([1, 2, 3, 4, 5])
y_train = np.array([2, 4, 5, 4, 5])

# Testing data
X_test = np.array([6, 7])

model = MeraLR()
model.fit(X_train, y_train)
predictions = model.predict(X_test)

print("Predictions:", predictions)


```


## using scikit learn
```python
from sklearn.linear_model import LinearRegression
import numpy as np

# Training data
X_train = np.array([1, 2, 3, 4, 5]).reshape(-1, 1)  # Reshape to 2D
y_train = np.array([2, 4, 5, 4, 5])

# Testing data
X_test = np.array([6, 7]).reshape(-1, 1)

# Model initialization and training
model = LinearRegression()
model.fit(X_train, y_train)

# Predictions
predictions = model.predict(X_test)

print("Slope (m):", model.coef_[0])
print("Intercept (b):", model.intercept_)
print("Predictions:", predictions)

```


## calculating errors and r2 score  and adjusted r2
```python
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score
import numpy as np

# Training data
X_train = np.array([1, 2, 3, 4, 5]).reshape(-1, 1)
y_train = np.array([2, 4, 5, 4, 5])

# Test data
X_test = np.array([6, 7]).reshape(-1, 1)
y_test = np.array([5, 6])

# Train model
model = LinearRegression()
model.fit(X_train, y_train)

# Predictions
y_pred = model.predict(X_test)

# Evaluation Metrics
mae = mean_absolute_error(y_test, y_pred)
mse = mean_squared_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)

# Adjusted R^2 Calculation
n = X_test.shape[0]  # number of observations
p = X_test.shape[1]  # number of predictors
adjusted_r2 = 1 - (1 - r2) * (n - 1) / (n - p - 1) if n > p + 1 else None

# Print Results
print("Mean Absolute Error (MAE):", mae)
print("Mean Squared Error (MSE):", mse)
print("R² Score:", r2)
print("Adjusted R² Score:", adjusted_r2)


```
