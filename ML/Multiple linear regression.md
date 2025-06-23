# 📘 Multiple Linear Regression (MLR)

## 📌 What is Multiple Linear Regression?

Multiple Linear Regression (MLR) is a **supervised learning** technique used to model the relationship between **one dependent variable** and **multiple independent variables**.

It’s an extension of **Simple Linear Regression**, which only uses one independent variable.

---

## 🧮 Regression Equation

The general form of the MLR equation is:

$$
y = \beta_0 + \beta_1x_1 + \beta_2x_2 + \cdots + \beta_nx_n + \varepsilon
$$

Where:
- $y$: Dependent variable (target)
- $x_1, x_2, \dots, x_n$: Independent variables (features)
- $\beta_0$: Intercept
- $\beta_1, \beta_2, \dots, \beta_n$: Coefficients
- $\varepsilon$: Error term

---

## 🔍 Assumptions of MLR

1. **Linearity**: The relationship between dependent and independent variables is linear.
2. **Independence**: Observations are independent.
3. **Homoscedasticity**: Constant variance of the error terms.
4. **Normality**: Error terms are normally distributed.
5. **No Multicollinearity**: Independent variables are not highly correlated.

---

## 🧠 Interpretation of Coefficients

Each coefficient $\beta_j$ represents the change in the target variable $y$ for a **one-unit increase in** $x_j$, holding all other variables constant.

---

## 🧪 Cost Function (Least Squares)

The cost function used in MLR is the **Sum of Squared Errors (SSE)**:

$$
J(\beta) = \sum_{i=1}^{n} (y_i - \hat{y}_i)^2
$$

Where:
- $\hat{y}_i = \beta_0 + \beta_1x_{i1} + \cdots + \beta_nx_{in}$  
- Goal: Minimize $J(\beta)$

---

## 📊 Evaluation Metrics

### 🔹 Mean Absolute Error (MAE)

$$
\text{MAE} = \frac{1}{n} \sum_{i=1}^{n} |y_i - \hat{y}_i|
$$

### 🔹 Mean Squared Error (MSE)

$$
\text{MSE} = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2
$$

### 🔹 Root Mean Squared Error (RMSE)

$$
\text{RMSE} = \sqrt{\text{MSE}}
$$

### 🔹 $R^2$ Score (Coefficient of Determination)

$$
R^2 = 1 - \frac{\sum (y_i - \hat{y}_i)^2}{\sum (y_i - \bar{y})^2}
$$

### 🔹 Adjusted $R^2$

Adjusted $R^2$ accounts for the number of features:

$$
\text{Adjusted } R^2 = 1 - \left( \frac{(1 - R^2)(n - 1)}{n - k - 1} \right)
$$

Where:
- $n$: Number of observations
- $k$: Number of predictors

---

## 💻 Python Implementation


```python
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score

model = LinearRegression()
model.fit(X_train, y_train)

y_pred = model.predict(X_test)

print("MAE:", mean_absolute_error(y_test, y_pred))
print("MSE:", mean_squared_error(y_test, y_pred))
print("RMSE:", mean_squared_error(y_test, y_pred, squared=False))
print("R2:", r2_score(y_test, y_pred))
```

##  FROM SCRATCH
```Python
class MeraLR:

    def __init__(self):

        self.coef_ = None

        self.intercept_ = None

    def fit(self,X_train,y_train):

        X_train = np.insert(X_train,0,1,axis=1)

        # calcuate the coeffs

        betas = np.linalg.inv(np.dot(X_train.T,X_train)).dot(X_train.T).dot(y_train)

        self.intercept_ = betas[0]

        self.coef_ = betas[1:]

    def predict(self,X_test):

        y_pred = np.dot(X_test,self.coef_) + self.intercept_

        return y_pred
```
