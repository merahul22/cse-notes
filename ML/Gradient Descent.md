```python
class GDRegressor:
    
    def __init__(self,learning_rate,epochs):
        self.m = 100
        self.b = -120
        self.lr = learning_rate
        self.epochs = epochs
        
    def fit(self,X,y):
        # calcualte the b using GD
        for i in range(self.epochs):
            loss_slope_b = -2 * np.sum(y - self.m*X.ravel() - self.b)
            loss_slope_m = -2 * np.sum((y - self.m*X.ravel() - self.b)*X.ravel())
            
            self.b = self.b - (self.lr * loss_slope_b)
            self.m = self.m - (self.lr * loss_slope_m)
        print(self.m,self.b)
        
    def predict(self,X):
        return self.m * X + self.b
```
> this one is from scratch -remember this  for logic 

## batch gradient descent with multiple variable 
```python
class GDRegressor:
    
    def __init__(self,learning_rate=0.01,epochs=100):
        
        self.coef_ = None
        self.intercept_ = None
        self.lr = learning_rate
        self.epochs = epochs
        
    def fit(self,X_train,y_train):
        # init your coefs
        self.intercept_ = 0
        self.coef_ = np.ones(X_train.shape[1])
        
        for i in range(self.epochs):
            # update all the coef and the intercept
            y_hat = np.dot(X_train,self.coef_) + self.intercept_
            #print("Shape of y_hat",y_hat.shape)
            intercept_der = -2 * np.mean(y_train - y_hat)
            self.intercept_ = self.intercept_ - (self.lr * intercept_der)
            
            coef_der = -2 * np.dot((y_train - y_hat),X_train)/X_train.shape[0]
            self.coef_ = self.coef_ - (self.lr * coef_der)
        
        print(self.intercept_,self.coef_)
    
    def predict(self,X_test):
        return np.dot(X_test,self.coef_) + self.intercept_
```
>here in intercept_der mean used as result is scaler ,while in coeff_der result is matrix so we need to get shape 

## stochastic gradient descent (from scratch)
```python
class SGDRegressor:
    
    def __init__(self,learning_rate=0.01,epochs=100):
        
        self.coef_ = None
        self.intercept_ = None
        self.lr = learning_rate
        self.epochs = epochs
        
    def fit(self,X_train,y_train):
        # init your coefs
        self.intercept_ = 0
        self.coef_ = np.ones(X_train.shape[1])
        
        for i in range(self.epochs):
            for j in range(X_train.shape[0]):
                idx = np.random.randint(0,X_train.shape[0])
                
                y_hat = np.dot(X_train[idx],self.coef_) + self.intercept_
                
                intercept_der = -2 * (y_train[idx] - y_hat)
                self.intercept_ = self.intercept_ - (self.lr * intercept_der)
                
                coef_der = -2 * np.dot((y_train[idx] - y_hat),X_train[idx])
                self.coef_ = self.coef_ - (self.lr * coef_der)
        
        print(self.intercept_,self.coef_)
    
    def predict(self,X_test):
        return np.dot(X_test,self.coef_) + self.intercept_
```
# using scikit learn(stochastic gradient descent)
```python
from sklearn.linear_model import SGDRegressor
from sklearn.datasets import make_regression
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import mean_squared_error

# 1. Generate dummy regression data
X, y = make_regression(n_samples=1000, n_features=10, noise=15, random_state=42)

# 2. Split into training and test
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 3. Feature scaling (very important for SGD to converge)
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)

# 4. Create SGD Regressor (true SGD happens when batch_size=1, default in older versions)
sgd = SGDRegressor(
    loss='squared_error',      # MSE loss
    learning_rate='constant',  # or 'invscaling', 'adaptive'
    eta0=0.01,                 # initial learning rate
    max_iter=100,              # number of passes over the training set
    tol=1e-3,                  # stopping criterion
    shuffle=True,              # shuffle data every epoch
    random_state=42,
    verbose=1
)

# 5. Train
sgd.fit(X_train, y_train)

# 6. Predict & Evaluate
y_pred = sgd.predict(X_test)
mse = mean_squared_error(y_test, y_pred)
print("Test MSE:", mse)


```

## mini batch gradient descent 
```python
import random

class MBGDRegressor:
    
    def __init__(self,batch_size,learning_rate=0.01,epochs=100):
        
        self.coef_ = None
        self.intercept_ = None
        self.lr = learning_rate
        self.epochs = epochs
        self.batch_size = batch_size
        
    def fit(self,X_train,y_train):
        # init your coefs
        self.intercept_ = 0
        self.coef_ = np.ones(X_train.shape[1])
        
        for i in range(self.epochs):
            
            for j in range(int(X_train.shape[0]/self.batch_size)):
                
                idx = random.sample(range(X_train.shape[0]),self.batch_size)
                
                y_hat = np.dot(X_train[idx],self.coef_) + self.intercept_
                #print("Shape of y_hat",y_hat.shape)
                intercept_der = -2 * np.mean(y_train[idx] - y_hat)
                self.intercept_ = self.intercept_ - (self.lr * intercept_der)

                coef_der = -2 * np.dot((y_train[idx] - y_hat),X_train[idx])
                self.coef_ = self.coef_ - (self.lr * coef_der)
        
        print(self.intercept_,self.coef_)
    
    def predict(self,X_test):
        return np.dot(X_test,self.coef_) + self.intercept_
```

## mini batch gradient descent (using scikit learn)

Scikit-learn **does not expose direct control** over batch size in its `LinearRegression` class — it uses a closed-form solution by default. However, **SGD-based models in scikit-learn**, like `SGDRegressor`, do support **mini-batch gradient descent** under the hood.

---

### ✅ How to Implement Mini-Batch Gradient Descent using `SGDRegressor`

```python
from sklearn.linear_model import SGDRegressor
from sklearn.datasets import make_regression
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split
import numpy as np

# Generate dummy regression data
X, y = make_regression(n_samples=1000, n_features=10, noise=10, random_state=42)

# Split and scale
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)

# Create SGDRegressor (uses mini-batches internally)
model = SGDRegressor(
    loss='squared_error',    # MSE Loss
    learning_rate='constant', 
    eta0=0.01,               # initial learning rate
    max_iter=100,            # number of epochs
    batch_size=32,           # 👈 Mini-batch size (default: 1, you can set this in newer versions)
    random_state=42,
    verbose=1
)

model.fit(X_train, y_train)

# Predict
y_pred = model.predict(X_test)

# Evaluate
from sklearn.metrics import mean_squared_error
print("Test MSE:", mean_squared_error(y_test, y_pred))
```

## batch gradient descent using scikit learn 
```python
from sklearn.linear_model import SGDRegressor
from sklearn.datasets import make_regression
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import mean_squared_error

# 1. Generate data
X, y = make_regression(n_samples=1000, n_features=10, noise=15, random_state=42)

# 2. Train-test split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 3. Scale features
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)

# 4. Create SGDRegressor to mimic **Batch Gradient Descent**
bgd = SGDRegressor(
    loss='squared_error',       # Linear regression
    learning_rate='constant',
    eta0=0.01,
    max_iter=100,               # Number of epochs
    tol=1e-3,
    shuffle=False,              # BGD doesn't shuffle each epoch
    batch_size=X_train.shape[0],# <- 👈 FULL batch per epoch
    random_state=42,
    verbose=1
)

# 5. Train and predict
bgd.fit(X_train, y_train)
y_pred = bgd.predict(X_test)

# 6. Evaluate
print("Test MSE:", mean_squared_error(y_test, y_pred))

```

## polynomial regression

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import PolynomialFeatures, StandardScaler
from sklearn.linear_model import LinearRegression
from sklearn.pipeline import Pipeline

class PolynomialRegression:
    """
    Polynomial Regression using a scikit-learn Pipeline.
    Handles train/test split, fitting, prediction, scoring, and plotting.
    """

    def __init__(self, degree, test_size=0.2, random_state=None):
        """
        Parameters:
        - degree: degree of the polynomial (integer)
        - test_size: fraction of data held out for testing (float)
        - random_state: seed for reproducibility (int or None)
        """
        self.degree = degree
        self.test_size = test_size
        self.random_state = random_state

        # Build a pipeline that:
        #  1) expands features into x, x^2, …, x^degree
        #  2) scales each feature to zero mean & unit variance
        #  3) fits a LinearRegression model on the transformed data
        self.pipeline = Pipeline([
            ("poly_features", PolynomialFeatures(degree=self.degree, include_bias=False)),
            ("scaler",       StandardScaler()),
            ("linear_model", LinearRegression())
        ])

        # Flag to ensure fit() is called before predict/score/plot
        self.is_fitted = False

    def fit(self, X, y):
        """
        - Splits X, y into train/test sets.
        - Fits the pipeline on the training data.
        """
        self.X_train, self.X_test, self.y_train, self.y_test = train_test_split(
            X, y,
            test_size=self.test_size,
            random_state=self.random_state
        )
        self.pipeline.fit(self.X_train, self.y_train)
        self.is_fitted = True

    def predict(self, X):
        """
        Returns model predictions on X.
        Raises an error if called before fit().
        """
        if not self.is_fitted:
            raise RuntimeError("You must call fit() before predict().")
        return self.pipeline.predict(X)

    def score(self):
        """
        Returns R² score on the held-out test set.
        Raises an error if called before fit().
        """
        if not self.is_fitted:
            raise RuntimeError("You must call fit() before score().")
        return self.pipeline.score(self.X_test, self.y_test)

    def plot(self, x_min=-3, x_max=3, num=100):
        """
        Plots:
         - The polynomial curve over [x_min, x_max].
         - The training points in one color.
         - The test points in another color.
        """
        if not self.is_fitted:
            raise RuntimeError("You must call fit() before plot().")

        # Generate a smooth grid of X values and predict
        X_new = np.linspace(x_min, x_max, num).reshape(-1, 1)
        y_new = self.predict(X_new)

        # Plot the regression curve
        plt.plot(X_new, y_new,
                 label=f"Degree {self.degree}",
                 linewidth=2)

        # Scatter plot of the train vs test points
        plt.scatter(self.X_train, self.y_train, alpha=0.6, label="Train")
        plt.scatter(self.X_test,  self.y_test,  alpha=0.6, label="Test")

        plt.xlabel("X")
        plt.ylabel("y")
        plt.title(f"Polynomial Regression (degree={self.degree})")
        plt.legend()
        plt.show()

```