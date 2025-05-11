# Question

Two classes have multivariate Gaussian distributions:

$\mu_1=[1,2]$, $\mu_2=[3,4]$, $\Sigma=I$, equal priors

Calculate the discriminant functions and classify $x=[2,3]$.

---

# Answer

The classes are modeled by multivariate Gaussian distributions $p(x \mid C_k) = \mathcal{N}(x \mid \mu_k, \Sigma_k)$. We are given $\mu_1 = [1, 2]^\top$, $\mu_2 = [3, 4]^\top$, and $\Sigma_1 = \Sigma_2 = \Sigma = I = \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix}$. The priors are equal, $P(C_1) = P(C_2)$.

For equal covariance matrices and equal priors, the classification rule based on maximizing the posterior probability $P(C_k \mid x)$ simplifies to choosing the class $k$ that maximizes the linear discriminant function:
$$
g_k(x) = x^\top \Sigma^{-1} \mu_k - \frac{1}{2} \mu_k^\top \Sigma^{-1} \mu_k
$$
Since $\Sigma = I$, we have $\Sigma^{-1} = I$.

**1. Calculate Discriminant Functions**

For Class 1 ($k=1$):
$\mu_1 = \begin{pmatrix} 1 \\ 2 \end{pmatrix}$
$\mu_1^\top \Sigma^{-1} \mu_1 = [1, 2] \begin{pmatrix} 1 \\ 2 \end{pmatrix} = (1)(1) + (2)(2) = 1 + 4 = 5$.
$x^\top \Sigma^{-1} \mu_1 = x^\top \begin{pmatrix} 1 \\ 2 \end{pmatrix} = [x_1, x_2] \begin{pmatrix} 1 \\ 2 \end{pmatrix} = x_1 + 2x_2$.
The discriminant function for Class 1 is:
$$
g_1(x) = (x_1 + 2x_2) - \frac{1}{2}(5) = x_1 + 2x_2 - 2.5
$$

For Class 2 ($k=2$):
$\mu_2 = \begin{pmatrix} 3 \\ 4 \end{pmatrix}$
$\mu_2^\top \Sigma^{-1} \mu_2 = [3, 4] \begin{pmatrix} 3 \\ 4 \end{pmatrix} = (3)(3) + (4)(4) = 9 + 16 = 25$.
$x^\top \Sigma^{-1} \mu_2 = x^\top \begin{pmatrix} 3 \\ 4 \end{pmatrix} = [x_1, x_2] \begin{pmatrix} 3 \\ 4 \end{pmatrix} = 3x_1 + 4x_2$.
The discriminant function for Class 2 is:
$$
g_2(x) = (3x_1 + 4x_2) - \frac{1}{2}(25) = 3x_1 + 4x_2 - 12.5
$$

The discriminant functions are $g_1(x) = x_1 + 2x_2 - 2.5$ and $g_2(x) = 3x_1 + 4x_2 - 12.5$.

**2. Classify $x=[2,3]$**

We evaluate the discriminant functions at $x=[2,3]$, where $x_1=2$ and $x_2=3$.

For Class 1:
$$
g_1([2,3]) = 2 + 2(3) - 2.5 = 2 + 6 - 2.5 = 8 - 2.5 = 5.5
$$

For Class 2:
$$
g_2([2,3]) = 3(2) + 4(3) - 12.5 = 6 + 12 - 12.5 = 18 - 12.5 = 5.5
$$

Since $g_1([2,3]) = g_2([2,3]) = 5.5$, the data point $x=[2,3]$ lies exactly on the decision boundary between the two classes. The classification is ambiguous based on this criterion alone.

The decision boundary is defined by $g_1(x) = g_2(x)$, which is $x_1 + 2x_2 - 2.5 = 3x_1 + 4x_2 - 12.5$, simplifying to $2x_1 + 2x_2 = 10$, or $x_1 + x_2 = 5$. Evaluating $x=[2,3]$ gives $2+3=5$, confirming it's on the boundary.

**Classification:**

The point $x=[2,3]$ lies on the decision boundary. Technically, it belongs to neither class uniquely according to the maximum likelihood/posterior rule with equal priors. In practice, a classifier might assign it to Class 1 by default in case of a tie, or it could be flagged as an ambiguous classification.

# Question

Given class priors and loss function:

$P(C_1)=0.4$, $P(C_2)=0.6$

Losses: $\lambda(1∣2)=5$, $\lambda(2∣1)=1$ (Assume $\lambda(1|1)=0, \lambda(2|2)=0$)

Determine the decision rule based on the risk minimization principle.

---

# Answer

The risk minimization principle dictates choosing the class that minimizes the expected risk for a given input $x$. For a two-class problem, the expected risk of classifying $x$ into class $C_i$ is given by:
$$
R(\alpha_i \mid x) = \sum_{j=1}^{2} \lambda(i \mid j) P(C_j \mid x)
$$
where $\lambda(i \mid j)$ is the loss incurred when deciding class $C_i$ but the true class is $C_j$, and $P(C_j \mid x)$ is the posterior probability of class $C_j$ given $x$.

Given losses are $\lambda(1∣2)=5$ and $\lambda(2∣1)=1$. We assume correct classification has zero loss, i.e., $\lambda(1|1)=0$ and $\lambda(2|2)=0$.

The expected risk for deciding class $C_1$ ($\alpha_1$) is:
$$
R(\alpha_1 \mid x) = \lambda(1 \mid 1) P(C_1 \mid x) + \lambda(1 \mid 2) P(C_2 \mid x) = 0 \cdot P(C_1 \mid x) + 5 \cdot P(C_2 \mid x) = 5 P(C_2 \mid x)
$$

The expected risk for deciding class $C_2$ ($\alpha_2$) is:
$$
R(\alpha_2 \mid x) = \lambda(2 \mid 1) P(C_1 \mid x) + \lambda(2 \mid 2) P(C_2 \mid x) = 1 \cdot P(C_1 \mid x) + 0 \cdot P(C_2 \mid x) = P(C_1 \mid x)
$$

According to the risk minimization principle, we choose class $C_1$ if $R(\alpha_1 \mid x) < R(\alpha_2 \mid x)$, and class $C_2$ otherwise.

The decision rule is:
Classify $x$ into $C_1$ if $5 P(C_2 \mid x) < P(C_1 \mid x)$
Classify $x$ into $C_2$ if $5 P(C_2 \mid x) > P(C_1 \mid x)$
The decision boundary is where $5 P(C_2 \mid x) = P(C_1 \mid x)$.

We can rewrite this using Bayes' Theorem, $P(C_j \mid x) = \frac{P(x \mid C_j) P(C_j)}{P(x)}$.
Substituting this into the inequality for choosing $C_1$:
$$
5 \frac{P(x \mid C_2) P(C_2)}{P(x)} < \frac{P(x \mid C_1) P(C_1)}{P(x)}
$$
Since $P(x) > 0$, we can multiply both sides by $P(x)$:
$$
5 P(x \mid C_2) P(C_2) < P(x \mid C_1) P(C_1)
$$
Now, substitute the given prior probabilities $P(C_1)=0.4$ and $P(C_2)=0.6$:
$$
5 P(x \mid C_2) (0.6) < P(x \mid C_1) (0.4)
$$
$$
3 P(x \mid C_2) < 0.4 P(x \mid C_1)
$$
To express the rule in terms of the likelihood ratio $\frac{P(x \mid C_1)}{P(x \mid C_2)}$, we rearrange the inequality (assuming $P(x \mid C_2) > 0$):
$$
\frac{P(x \mid C_1)}{P(x \mid C_2)} > \frac{3}{0.4}
$$
$$
\frac{P(x \mid C_1)}{P(x \mid C_2)} > 7.5
$$

Thus, the decision rule based on the risk minimization principle is:

-   Classify $x$ into $C_1$ if $\frac{P(x \mid C_1)}{P(x \mid C_2)} > 7.5$
-   Classify $x$ into $C_2$ if $\frac{P(x \mid C_1)}{P(x \mid C_2)} < 7.5$
-   If $\frac{P(x \mid C_1)}{P(x \mid C_2)} = 7.5$, the decision is on the boundary.

This threshold (7.5) is equal to $\frac{\lambda(1|2) P(C_2)}{\lambda(2|1) P(C_1)}$.


You are given a partially observed data point:
$x = [\ ?, 6]$

# You are given a partially observed data point: $x = [\ ?, 6]$ There are two possible classes, $C_1$ and $C_2$, each with known Gaussian distributions:

**Class $C_1$:**
* Mean vector: $\mu_1=[4, 6]^\top$
* Variance vector: $\sigma_1^2 = [1, 4]$ (implies diagonal covariance $\Sigma_1 = \begin{pmatrix} 1 & 0 \\ 0 & 4 \end{pmatrix}$)
* Prior probability: $P(C_1)=0.7$

**Class $C_2$:**
* Mean vector: $\mu_2=[2, 7]^\top$
* Variance vector: $\sigma_2^2 = [1, 1]$ (implies diagonal covariance $\Sigma_2 = \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix} = I$)
* Prior probability: $P(C_2)=0.3$

---

**Calculations:**

**1. Estimate the missing value**

We estimate the missing value in the first feature ($x_1$) using the mean of that feature for each class:

* For $C_1$: Estimated $x_1$ is $\mu_{1,1} = 4$. The estimated complete data point is $\hat{x}^{(1)} = [4, 6]^\top$.
* For $C_2$: Estimated $x_1$ is $\mu_{2,1} = 2$. The estimated complete data point is $\hat{x}^{(2)} = [2, 6]^\top$.

**2. Compute the likelihood of the estimated complete data point**

We use the multivariate Gaussian PDF $p(x \mid \mu, \Sigma) = \frac{1}{\sqrt{(2\pi)^d |\Sigma|}} \exp\left(-\frac{1}{2}(x - \mu)^\top \Sigma^{-1} (x - \mu)\right)$. For diagonal $\Sigma$, this simplifies to $p(x \mid \mu, \Sigma) = \frac{1}{\sqrt{(2\pi)^d \prod \sigma_j^2}} \exp\left(-\frac{1}{2} \sum \frac{(x_j - \mu_j)^2}{\sigma_j^2}\right)$. Here, $d=2$.

* **For $C_1$** (using $\hat{x}^{(1)} = [4, 6]^\top$, $\mu_1 = [4, 6]^\top$, $\sigma_1^2 = [1, 4]$):
    * Determinant: $|\Sigma_1| = 1 \times 4 = 4$.
    * Mahalanobis distance term in exponent:
        $\sum_{j=1}^2 \frac{(\hat{x}^{(1)}_j - \mu_{1,j})^2}{\sigma_{1,j}^2} = \frac{(4 - 4)^2}{1} + \frac{(6 - 6)^2}{4} = \frac{0^2}{1} + \frac{0^2}{4} = 0$.
    * Likelihood:
        $$
        P(\hat{x}^{(1)} \mid C_1) = \frac{1}{\sqrt{(2\pi)^2 \cdot 4}} \exp\left(-\frac{1}{2} \cdot 0\right) = \frac{1}{\sqrt{4\pi^2 \cdot 4}} \exp(0) = \frac{1}{\sqrt{16\pi^2}} = \frac{1}{4\pi}
        $$

* **For $C_2$** (using $\hat{x}^{(2)} = [2, 6]^\top$, $\mu_2 = [2, 7]^\top$, $\sigma_2^2 = [1, 1]$):
    * Determinant: $|\Sigma_2| = 1 \times 1 = 1$.
    * Mahalanobis distance term in exponent:
        $\sum_{j=1}^2 \frac{(\hat{x}^{(2)}_j - \mu_{2,j})^2}{\sigma_{2,j}^2} = \frac{(2 - 2)^2}{1} + \frac{(6 - 7)^2}{1} = \frac{0^2}{1} + \frac{(-1)^2}{1} = 0 + 1 = 1$.
    * Likelihood:
        $$
        P(\hat{x}^{(2)} \mid C_2) = \frac{1}{\sqrt{(2\pi)^2 \cdot 1}} \exp\left(-\frac{1}{2} \cdot 1\right) = \frac{1}{\sqrt{4\pi^2}} \exp(-0.5) = \frac{1}{2\pi} \exp(-0.5)
        $$

**3. Multiply likelihood by class prior (Unnormalized Posteriors)**

* Unnormalized Posterior for $C_1$:
    $$
    P(\hat{x}^{(1)} \mid C_1) P(C_1) = \frac{1}{4\pi} \times 0.7 = \frac{0.7}{4\pi}
    $$
* Unnormalized Posterior for $C_2$:
    $$
    P(\hat{x}^{(2)} \mid C_2) P(C_2) = \left(\frac{1}{2\pi} \exp(-0.5)\right) \times 0.3 = \frac{0.3}{2\pi} \exp(-0.5)
    $$

Let's approximate the values: $\pi \approx 3.14159$, $\exp(-0.5) \approx 0.60653$.
* Unnormalized Posterior for $C_1 \approx \frac{0.7}{4 \times 3.14159} \approx \frac{0.7}{12.56636} \approx 0.0557$
* Unnormalized Posterior for $C_2 \approx \frac{0.3}{2 \times 3.14159} \times 0.60653 \approx \frac{0.3}{6.28318} \times 0.60653 \approx 0.04775 \times 0.60653 \approx 0.02898$

**4. MAP (Maximum A Posteriori) Rule**

We compare the unnormalized posteriors:
$0.0557$ (for $C_1$) vs $0.02898$ (for $C_2$)

Since $0.0557 > 0.02898$, the unnormalized posterior for $C_1$ is higher.

**Classification:**

According to the MAP rule, the point $x=[\ ?, 6]$ is classified into **Class $C_1$**.

# Estimating Mean ($\mu$) with Gaussian Likelihood and Uniform Prior

You are given data $x = [4, 5, 6]$ from a Gaussian distribution with known variance $\sigma^2 = 1$.
The prior distribution for the mean $\mu$ is uniform between $[0, 10]$.

You need to compute the Maximum Likelihood Estimate (MLE) and the Maximum A Posteriori (MAP) estimate for $\mu$.

---

## Solution

Let the data points be $x_1=4, x_2=5, x_3=6$. The number of data points is $n=3$.
The likelihood of a single data point $x_i$ given $\mu$ and $\sigma^2$ is $P(x_i \mid \mu, \sigma^2) = \frac{1}{\sqrt{2\pi\sigma^2}} \exp\left(-\frac{(x_i - \mu)^2}{2\sigma^2}\right)$.

The likelihood of the data $x = [x_1, x_2, x_3]$ is the product of individual likelihoods (since the data points are assumed independent):
$$
L(\mu) = P(x \mid \mu, \sigma^2) = \prod_{i=1}^n P(x_i \mid \mu, \sigma^2)
$$

The prior distribution for $\mu$ is uniform on $[0, 10]$:
$$
P(\mu) = \begin{cases} \frac{1}{10} & \text{if } 0 \le \mu \le 10 \\ 0 & \text{otherwise} \end{cases}
$$

### Maximum Likelihood Estimate (MLE)

The MLE for $\mu$ is the value that maximizes the likelihood $L(\mu)$. This is equivalent to maximizing the log-likelihood $\ell(\mu) = \log L(\mu)$:
$$
\ell(\mu) = \sum_{i=1}^n \log P(x_i \mid \mu, \sigma^2) = \sum_{i=1}^n \left[ -\frac{1}{2}\log(2\pi\sigma^2) - \frac{(x_i - \mu)^2}{2\sigma^2} \right]
$$
Since $\sigma^2=1$ is fixed, maximizing $\ell(\mu)$ with respect to $\mu$ is equivalent to minimizing $\sum_{i=1}^n (x_i - \mu)^2$. The value of $\mu$ that minimizes the sum of squared differences to a set of points is the sample mean.

The MLE estimate $\hat{\mu}_{MLE}$ is the sample mean of the data:
$$
\hat{\mu}_{MLE} = \frac{1}{n} \sum_{i=1}^n x_i
$$
Given $x = [4, 5, 6]$ and $n=3$:
$$
\hat{\mu}_{MLE} = \frac{4 + 5 + 6}{3} = \frac{15}{3} = 5
$$

### Maximum A Posteriori (MAP) Estimate

The MAP estimate for $\mu$ is the value that maximizes the posterior probability $P(\mu \mid x)$. Using Bayes' Theorem, $P(\mu \mid x) \propto P(x \mid \mu) P(\mu) = L(\mu) P(\mu)$.
Maximizing $L(\mu) P(\mu)$ is equivalent to maximizing $\log(L(\mu) P(\mu)) = \log L(\mu) + \log P(\mu) = \ell(\mu) + \log P(\mu)$.

The log-prior is:
$$
\log P(\mu) = \begin{cases} \log\left(\frac{1}{10}\right) & \text{if } 0 \le \mu \le 10 \\ -\infty & \text{otherwise} \end{cases}
$$

To maximize $\ell(\mu) + \log P(\mu)$, we need $\mu$ to be in the range $[0, 10]$, otherwise the posterior is 0 (or $\log P(\mu) = -\infty$). Within the range $[0, 10]$, $\log P(\mu) = \log(1/10)$, which is a constant with respect to $\mu$.

Therefore, for $0 \le \mu \le 10$, we maximize:
$$
\ell(\mu) + \log\left(\frac{1}{10}\right)
$$
Maximizing this expression within the interval $[0, 10]$ is equivalent to maximizing $\ell(\mu)$ within the interval $[0, 10]$.

The unconstrained maximum of $\ell(\mu)$ is the MLE estimate $\hat{\mu}_{MLE} = 5$. We check if this value falls within the support of the prior, $[0, 10]$.
Since $0 \le 5 \le 10$, the unconstrained maximum lies within the allowed range for $\mu$. The MAP estimate is therefore the same as the MLE estimate.

$$
\hat{\mu}_{MAP} = \hat{\mu}_{MLE} = 5
$$

If the MLE had fallen outside the range $[0, 10]$ (e.g., if the sample mean was 12), the MAP estimate would be the closest value within the range that maximizes $\ell(\mu)$ on the boundary, which would be 10 in that example. If the sample mean was -2, the MAP estimate would be 0.

In this case, the sample mean $5$ is within the prior interval $[0, 10]$.

**Summary:**

* The MLE estimate for $\mu$ is the sample mean, $\hat{\mu}_{MLE} = 5$.
* The MAP estimate for $\mu$ maximizes the likelihood times the prior. Due to the uniform prior, this is equivalent to maximizing the likelihood within the prior's support $[0, 10]$.
* Since the MLE estimate ($5$) falls within the prior's support $[0, 10]$, the MAP estimate is equal to the MLE estimate.

The MAP and MLE estimates for $\mu$ are both $5$.

# Question

Given the 2D dataset: $x=[(2,0), (0,2), (3,1)]$

Calculate the following for PCA projection:

* Mean center the data.
* Compute the covariance matrix.
* Find the eigenvalues and eigenvectors.
* Identify the first principal component.
* Project the original data onto this component.

---

# Answer

The given 2D dataset is $X = \{x^{(1)}, x^{(2)}, x^{(3)}\}$, where $x^{(1)} = \begin{pmatrix} 2 \\ 0 \end{pmatrix}$, $x^{(2)} = \begin{pmatrix} 0 \\ 2 \end{pmatrix}$, and $x^{(3)} = \begin{pmatrix} 3 \\ 1 \end{pmatrix}$. The number of data points is $n=3$.

**1. Mean Center the Data**

First, calculate the mean vector of the dataset:
$$
\bar{x} = \frac{1}{n} \sum_{i=1}^n x^{(i)} = \frac{1}{3} \left( \begin{pmatrix} 2 \\ 0 \end{pmatrix} + \begin{pmatrix} 0 \\ 2 \end{pmatrix} + \begin{pmatrix} 3 \\ 1 \end{pmatrix} \right) = \frac{1}{3} \begin{pmatrix} 2+0+3 \\ 0+2+1 \end{pmatrix} = \frac{1}{3} \begin{pmatrix} 5 \\ 3 \end{pmatrix} = \begin{pmatrix} 5/3 \\ 1 \end{pmatrix}
$$
Now, subtract the mean vector from each data point to center the data:
$$
x_c^{(1)} = x^{(1)} - \bar{x} = \begin{pmatrix} 2 \\ 0 \end{pmatrix} - \begin{pmatrix} 5/3 \\ 1 \end{pmatrix} = \begin{pmatrix} 2 - 5/3 \\ 0 - 1 \end{pmatrix} = \begin{pmatrix} 1/3 \\ -1 \end{pmatrix}
$$
$$
x_c^{(2)} = x^{(2)} - \bar{x} = \begin{pmatrix} 0 \\ 2 \end{pmatrix} - \begin{pmatrix} 5/3 \\ 1 \end{pmatrix} = \begin{pmatrix} 0 - 5/3 \\ 2 - 1 \end{pmatrix} = \begin{pmatrix} -5/3 \\ 1 \end{pmatrix}
$$
$$
x_c^{(3)} = x^{(3)} - \bar{x} = \begin{pmatrix} 3 \\ 1 \end{pmatrix} - \begin{pmatrix} 5/3 \\ 1 \end{pmatrix} = \begin{pmatrix} 3 - 5/3 \\ 1 - 1 \end{pmatrix} = \begin{pmatrix} 4/3 \\ 0 \end{pmatrix}
$$
The mean-centered data points are $\left\{ \begin{pmatrix} 1/3 \\ -1 \end{pmatrix}, \begin{pmatrix} -5/3 \\ 1 \end{pmatrix}, \begin{pmatrix} 4/3 \\ 0 \end{pmatrix} \right\}$.

**2. Compute the Covariance Matrix**

The covariance matrix $\Sigma$ for the 2D mean-centered data is calculated as $\Sigma = \frac{1}{n-1} \sum_{i=1}^n x_c^{(i)} (x_c^{(i)})^\top$. Here $n-1 = 3-1=2$.
$$
\Sigma = \frac{1}{2} \left( \begin{pmatrix} 1/3 \\ -1 \end{pmatrix}\begin{pmatrix} 1/3 & -1 \end{pmatrix} + \begin{pmatrix} -5/3 \\ 1 \end{pmatrix}\begin{pmatrix} -5/3 & 1 \end{pmatrix} + \begin{pmatrix} 4/3 \\ 0 \end{pmatrix}\begin{pmatrix} 4/3 & 0 \end{pmatrix} \right)
$$
$$
\Sigma = \frac{1}{2} \left( \begin{pmatrix} 1/9 & -1/3 \\ -1/3 & 1 \end{pmatrix} + \begin{pmatrix} 25/9 & -5/3 \\ -5/3 & 1 \end{pmatrix} + \begin{pmatrix} 16/9 & 0 \\ 0 & 0 \end{pmatrix} \right)
$$
$$
\Sigma = \frac{1}{2} \begin{pmatrix} 1/9 + 25/9 + 16/9 & -1/3 - 5/3 + 0 \\ -1/3 - 5/3 + 0 & 1 + 1 + 0 \end{pmatrix} = \frac{1}{2} \begin{pmatrix} 42/9 & -6/3 \\ -6/3 & 2 \end{pmatrix} = \frac{1}{2} \begin{pmatrix} 14/3 & -2 \\ -2 & 2 \end{pmatrix}
$$
$$
\Sigma = \begin{pmatrix} 7/3 & -1 \\ -1 & 1 \end{pmatrix}
$$

**3. Find the Eigenvalues and Eigenvectors**

To find the eigenvalues $\lambda$, we solve the characteristic equation $\det(\Sigma - \lambda I) = 0$:
$$
\det \begin{pmatrix} 7/3 - \lambda & -1 \\ -1 & 1 - \lambda \end{pmatrix} = (7/3 - \lambda)(1 - \lambda) - (-1)(-1) = 0
$$
$$
(7/3 - 7\lambda/3 - \lambda + \lambda^2) - 1 = 0
$$
$$
\lambda^2 - (7/3 + 1)\lambda + (7/3 - 1) = 0
$$
$$
\lambda^2 - \frac{10}{3}\lambda + \frac{4}{3} = 0
$$
Multiply by 3: $3\lambda^2 - 10\lambda + 4 = 0$.
Using the quadratic formula $\lambda = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}$:
$$
\lambda = \frac{10 \pm \sqrt{(-10)^2 - 4(3)(4)}}{2(3)} = \frac{10 \pm \sqrt{100 - 48}}{6} = \frac{10 \pm \sqrt{52}}{6} = \frac{10 \pm 2\sqrt{13}}{6} = \frac{5 \pm \sqrt{13}}{3}
$$
The eigenvalues are $\lambda_1 = \frac{5 + \sqrt{13}}{3}$ and $\lambda_2 = \frac{5 - \sqrt{13}}{3}$.

To find the eigenvectors $v = \begin{pmatrix} v_1 \\ v_2 \end{pmatrix}$, we solve $(\Sigma - \lambda I)v = 0$ for each eigenvalue.

For $\lambda_1 = \frac{5 + \sqrt{13}}{3}$:
$$
\begin{pmatrix} 7/3 - \frac{5 + \sqrt{13}}{3} & -1 \\ -1 & 1 - \frac{5 + \sqrt{13}}{3} \end{pmatrix} \begin{pmatrix} v_1 \\ v_2 \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \end{pmatrix}
$$
$$
\begin{pmatrix} \frac{2 - \sqrt{13}}{3} & -1 \\ -1 & \frac{-2 - \sqrt{13}}{3} \end{pmatrix} \begin{pmatrix} v_1 \\ v_2 \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \end{pmatrix}
$$
From the first row: $\frac{2 - \sqrt{13}}{3} v_1 - v_2 = 0 \implies v_2 = \frac{2 - \sqrt{13}}{3} v_1$.
Choosing $v_1 = 3$, we get $v_2 = 2 - \sqrt{13}$. An eigenvector is $\begin{pmatrix} 3 \\ 2 - \sqrt{13} \end{pmatrix}$.

For $\lambda_2 = \frac{5 - \sqrt{13}}{3}$:
$$
\begin{pmatrix} 7/3 - \frac{5 - \sqrt{13}}{3} & -1 \\ -1 & 1 - \frac{5 - \sqrt{13}}{3} \end{pmatrix} \begin{pmatrix} v_1 \\ v_2 \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \end{pmatrix}
$$
$$
\begin{pmatrix} \frac{2 + \sqrt{13}}{3} & -1 \\ -1 & \frac{-2 + \sqrt{13}}{3} \end{pmatrix} \begin{pmatrix} v_1 \\ v_2 \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \end{pmatrix}
$$
From the first row: $\frac{2 + \sqrt{13}}{3} v_1 - v_2 = 0 \implies v_2 = \frac{2 + \sqrt{13}}{3} v_1$.
Choosing $v_1 = 3$, we get $v_2 = 2 + \sqrt{13}$. An eigenvector is $\begin{pmatrix} 3 \\ 2 + \sqrt{13} \end{pmatrix}$.

**4. Identify the First Principal Component**

The largest eigenvalue is $\lambda_1 = \frac{5 + \sqrt{13}}{3}$ ($\approx 2.869$).
The corresponding eigenvector is $\begin{pmatrix} 3 \\ 2 - \sqrt{13} \end{pmatrix}$.
To use it for projection, we usually normalize it to a unit vector. The squared magnitude is $3^2 + (2-\sqrt{13})^2 = 9 + (4 - 4\sqrt{13} + 13) = 26 - 4\sqrt{13}$.
The first principal component (unit eigenvector $v_1$) is:
$$
v_1 = \frac{1}{\sqrt{26 - 4\sqrt{13}}} \begin{pmatrix} 3 \\ 2 - \sqrt{13} \end{pmatrix}
$$

**5. Project the Original Data onto this Component**

We project the mean-centered data points ($x_c^{(i)}$) onto the first principal component vector ($v_1$). The projection $y^{(i)}$ is the dot product $(x_c^{(i)})^\top v_1$.

$$
y^{(1)} = (x_c^{(1)})^\top v_1 = \begin{pmatrix} 1/3 & -1 \end{pmatrix} \frac{1}{\sqrt{26 - 4\sqrt{13}}} \begin{pmatrix} 3 \\ 2 - \sqrt{13} \end{pmatrix} = \frac{1}{\sqrt{26 - 4\sqrt{13}}} \left( (1/3)(3) + (-1)(2 - \sqrt{13}) \right) = \frac{1 - 2 + \sqrt{13}}{\sqrt{26 - 4\sqrt{13}}} = \frac{\sqrt{13} - 1}{\sqrt{26 - 4\sqrt{13}}}
$$
$$
y^{(2)} = (x_c^{(2)})^\top v_1 = \begin{pmatrix} -5/3 & 1 \end{pmatrix} \frac{1}{\sqrt{26 - 4\sqrt{13}}} \begin{pmatrix} 3 \\ 2 - \sqrt{13} \end{pmatrix} = \frac{1}{\sqrt{26 - 4\sqrt{13}}} \left( (-5/3)(3) + (1)(2 - \sqrt{13}) \right) = \frac{-5 + 2 - \sqrt{13}}{\sqrt{26 - 4\sqrt{13}}} = \frac{-3 - \sqrt{13}}{\sqrt{26 - 4\sqrt{13}}}
$$
$$
y^{(3)} = (x_c^{(3)})^\top v_1 = \begin{pmatrix} 4/3 & 0 \end{pmatrix} \frac{1}{\sqrt{26 - 4\sqrt{13}}} \begin{pmatrix} 3 \\ 2 - \sqrt{13} \end{pmatrix} = \frac{1}{\sqrt{26 - 4\sqrt{13}}} \left( (4/3)(3) + (0)(2 - \sqrt{13}) \right) = \frac{4 + 0}{\sqrt{26 - 4\sqrt{13}}} = \frac{4}{\sqrt{26 - 4\sqrt{13}}}
$$

The projected data points onto the first principal component are the scalar values: $\left\{ \frac{\sqrt{13} - 1}{\sqrt{26 - 4\sqrt{13}}}, \frac{-3 - \sqrt{13}}{\sqrt{26 - 4\sqrt{13}}}, \frac{4}{\sqrt{26 - 4\sqrt{13}}} \right\}$.
# Question

Given a data point $x=6$, cluster centers $c_1=4$, $c_2=10$, and fuzziness parameter $m=2$, compute the membership values $u_1$ and $u_2$.

---

# Answer

We are given:
- Data point $x = 6$
- Cluster centers $c_1 = 4$ and $c_2 = 10$
- Fuzziness parameter $m = 2$

We need to compute the fuzzy membership values $u_1$ (membership in cluster 1) and $u_2$ (membership in cluster 2) for the data point $x$.

The formula for computing the membership $u_{ik}$ of a data point $x_i$ in cluster $k$ is:
$$
u_{ik} = \frac{1}{\sum_{j=1}^{K} \left(\frac{\|x_i - c_k\|}{\|x_i - c_j\|}\right)^{\frac{2}{m-1}}}
$$
In our case, we have one data point $x_i=x=6$ and $K=2$ clusters. The exponent is $\frac{2}{m-1} = \frac{2}{2-1} = \frac{2}{1} = 2$. The formula simplifies for our specific point $x$ and clusters $C_1, C_2$:
$$
u_k = \frac{1}{\sum_{j=1}^{2} \left(\frac{\|x - c_k\|}{\|x - c_j\|}\right)^{2}}
$$

First, calculate the distances from $x$ to each cluster center:
$d_1 = \|x - c_1\| = \|6 - 4\| = \|2\| = 2$
$d_2 = \|x - c_2\| = \|6 - 10\| = \|-4\| = 4$

Now, compute the membership for $u_1$ (membership in cluster 1, $k=1$):
$$
u_1 = \frac{1}{\left(\frac{\|x - c_1\|}{\|x - c_1\|}\right)^{2} + \left(\frac{\|x - c_1\|}{\|x - c_2\|}\right)^{2}} = \frac{1}{\left(\frac{d_1}{d_1}\right)^{2} + \left(\frac{d_1}{d_2}\right)^{2}}
$$
Substitute the distances:
$$
u_1 = \frac{1}{(1)^{2} + \left(\frac{2}{4}\right)^{2}} = \frac{1}{1 + \left(\frac{1}{2}\right)^{2}} = \frac{1}{1 + \frac{1}{4}} = \frac{1}{\frac{4+1}{4}} = \frac{1}{\frac{5}{4}} = \frac{4}{5}
$$

Next, compute the membership for $u_2$ (membership in cluster 2, $k=2$):
$$
u_2 = \frac{1}{\left(\frac{\|x - c_2\|}{\|x - c_1\|}\right)^{2} + \left(\frac{\|x - c_2\|}{\|x - c_2\|}\right)^{2}} = \frac{1}{\left(\frac{d_2}{d_1}\right)^{2} + \left(\frac{d_2}{d_2}\right)^{2}}
$$
Substitute the distances:
$$
u_2 = \frac{1}{\left(\frac{4}{2}\right)^{2} + (1)^{2}} = \frac{1}{(2)^{2} + 1} = \frac{1}{4 + 1} = \frac{1}{5}
$$

Alternatively, since the sum of memberships for a data point across all clusters must equal 1 ($\sum_{k=1}^K u_k = 1$), we could calculate $u_2$ as $u_2 = 1 - u_1 = 1 - \frac{4}{5} = \frac{1}{5}$.

The membership values are $u_1 = \frac{4}{5}$ and $u_2 = \frac{1}{5}$.

The final membership values for data point $x=6$ are $u_1 = 0.8$ and $u_2 = 0.2$.

# Question

Given a simple 2-layer neural network with weights and biases, compute the final output for a specific input vector. Explain with the help of an example.

---

# Answer

In a simple 2-layer neural network (meaning one hidden layer and one output layer, besides the input layer), the input vector passes through the hidden layer and then the output layer. Each layer performs a linear transformation (weighted sum plus bias) followed by a non-linear activation function.

Let's define a simple example network and trace the computation:

**Example Network:**

* **Input Layer:** 2 neurons ($x_1, x_2$)
* **Hidden Layer:** 2 neurons ($h_1, h_2$)
* **Output Layer:** 1 neuron ($\hat{y}$)

* **Input Vector:** $x = \begin{pmatrix} 0.5 \\ 1.0 \end{pmatrix}$

* **Weights and Biases for the Hidden Layer (Layer 1):**
    Weights $W^{(1)}$ (size: 2x2), Biases $b^{(1)}$ (size: 2x1)
    $$
    W^{(1)} = \begin{pmatrix} 0.1 & 0.4 \\ 0.2 & 0.5 \end{pmatrix}, \quad b^{(1)} = \begin{pmatrix} 0.1 \\ 0.2 \end{pmatrix}
    $$

* **Weights and Biases for the Output Layer (Layer 2):**
    Weights $W^{(2)}$ (size: 1x2), Biases $b^{(2)}$ (size: 1x1)
    $$
    W^{(2)} = \begin{pmatrix} 0.3 & 0.6 \end{pmatrix}, \quad b^{(2)} = \begin{pmatrix} 0.3 \end{pmatrix}
    $$

* **Activation Function:** We will use the Sigmoid function, $\sigma(z) = \frac{1}{1 + e^{-z}}$, for both the hidden and output layers.

**Computation Steps (Forward Pass):**

**Step 1: Compute the output of the Hidden Layer**

First, we calculate the weighted sum of the input vector $x$ using the weights $W^{(1)}$ and add the bias $b^{(1)}$. This gives us the pre-activation values for the hidden layer, $z^{(1)}$.
$$
z^{(1)} = W^{(1)}x + b^{(1)}
$$
$$
z^{(1)} = \begin{pmatrix} 0.1 & 0.4 \\ 0.2 & 0.5 \end{pmatrix} \begin{pmatrix} 0.5 \\ 1.0 \end{pmatrix} + \begin{pmatrix} 0.1 \\ 0.2 \end{pmatrix}
$$
Perform the matrix-vector multiplication:
$$
W^{(1)}x = \begin{pmatrix} (0.1)(0.5) + (0.4)(1.0) \\ (0.2)(0.5) + (0.5)(1.0) \end{pmatrix} = \begin{pmatrix} 0.05 + 0.4 \\ 0.1 + 0.5 \end{pmatrix} = \begin{pmatrix} 0.45 \\ 0.6 \end{pmatrix}
$$
Now, add the bias vector:
$$
z^{(1)} = \begin{pmatrix} 0.45 \\ 0.6 \end{pmatrix} + \begin{pmatrix} 0.1 \\ 0.2 \end{pmatrix} = \begin{pmatrix} 0.55 \\ 0.8 \end{pmatrix}
$$
Next, apply the activation function (Sigmoid) to each element of $z^{(1)}$ to get the activated outputs of the hidden layer, $a^{(1)}$:
$$
a^{(1)} = \sigma(z^{(1)}) = \sigma\left(\begin{pmatrix} 0.55 \\ 0.8 \end{pmatrix}\right) = \begin{pmatrix} \sigma(0.55) \\ \sigma(0.8) \end{pmatrix}
$$
Calculate the sigmoid values:
$\sigma(0.55) = \frac{1}{1 + e^{-0.55}} \approx \frac{1}{1 + 0.5769} \approx 0.634$
$\sigma(0.8) = \frac{1}{1 + e^{-0.8}} \approx \frac{1}{1 + 0.4493} \approx 0.690$
$$
a^{(1)} \approx \begin{pmatrix} 0.634 \\ 0.690 \end{pmatrix}
$$

**Step 2: Compute the final output of the Output Layer**

Now, we use the activated outputs of the hidden layer ($a^{(1)}$) as input to the output layer. We calculate the weighted sum using weights $W^{(2)}$ and add the bias $b^{(2)}$. This gives us the pre-activation value for the output layer, $z^{(2)}$.
$$
z^{(2)} = W^{(2)}a^{(1)} + b^{(2)}
$$
$$
z^{(2)} = \begin{pmatrix} 0.3 & 0.6 \end{pmatrix} \begin{pmatrix} 0.634 \\ 0.690 \end{pmatrix} + \begin{pmatrix} 0.3 \end{pmatrix}
$$
Perform the matrix-vector multiplication (which is a dot product here):
$$
W^{(2)}a^{(1)} = (0.3)(0.634) + (0.6)(0.690) = 0.1902 + 0.4140 = 0.6042
$$
Now, add the bias:
$$
z^{(2)} = 0.6042 + 0.3 = 0.9042
$$
Finally, apply the activation function (Sigmoid) to $z^{(2)}$ to get the network's final output, $\hat{y}$:
$$
\hat{y} = \sigma(z^{(2)}) = \sigma(0.9042)
$$
Calculate the sigmoid value:
$$
\hat{y} = \frac{1}{1 + e^{-0.9042}} \approx \frac{1}{1 + 0.4049} \approx 0.712
$$

The final output of the network for the input vector $\begin{pmatrix} 0.5 \\ 1.0 \end{pmatrix}$ is approximately $0.712$.

This process of calculating the output by moving forward through the layers is called the **forward pass**. Each layer computes its output based on the outputs of the previous layer, using its weights, biases, and activation function.

# Question( refer video for backpropogation)

For a given neuron output and target, calculate the error and update the weights using gradient descent with a given learning rate. Explain with the help of an example.

---

# Answer

To calculate the error for a single neuron and update its weights using gradient descent, we follow these steps based on a defined loss function and learning rate. **Scenario:** Consider a single neuron with 2 inputs, $x_1$ and $x_2$, corresponding weights $w_1$ and $w_2$, and a bias $b$. The neuron computes a pre-activation value $z$ as a weighted sum: $$ z = w_1 x_1 + w_2 x_2 + b $$ Let's assume a linear activation function, so the neuron's output is $\hat{y} = z$. We compare this output to a target value $y$ using the Mean Squared Error (MSE) loss function: $$ L = \frac{1}{2}(y - \hat{y})^2 $$ We will use gradient descent to update the weights ($w_1, w_2$) and bias ($b$) to minimize $L$. **Example Values:** * Input: $x = \begin{pmatrix} x_1 \\ x_2 \end{pmatrix} = \begin{pmatrix} 1.0 \\ 2.0 \end{pmatrix}$ * Initial Weights: $w = \begin{pmatrix} w_1 \\ w_2 \end{pmatrix} = \begin{pmatrix} 0.5 \\ -0.1 \end{pmatrix}$ * Initial Bias: $b = 0.2$ * Target Output: $y = 0.8$ * Learning Rate: $\eta = 0.1$ **Steps:** **1. Forward Pass: Compute Output ($\hat{y}$) and Error ($L$)** Calculate the neuron's output using the current parameters: $$ z = w_1 x_1 + w_2 x_2 + b = (0.5)(1.0) + (-0.1)(2.0) + 0.2 $$ $$ z = 0.5 - 0.2 + 0.2 = 0.5 $$ Since $\hat{y} = z$, the output is $\hat{y} = 0.5$. Calculate the loss (error) using the MSE function: $$ L = \frac{1}{2}(y - \hat{y})^2 = \frac{1}{2}(0.8 - 0.5)^2 $$ $$ L = \frac{1}{2}(0.3)^2 = \frac{1}{2}(0.09) = 0.045 $$ **2. Backpropagation: Compute Gradients ($\nabla L$)** To update the parameters using gradient descent, we need the gradients of the loss with respect to each parameter ($\frac{\partial L}{\partial w_1}, \frac{\partial L}{\partial w_2}, \frac{\partial L}{\partial b}$). We use the chain rule. The chain rule gives us the gradient with respect to the pre-activation $z$: $$ \frac{\partial L}{\partial z} = \frac{\partial L}{\partial \hat{y}} \frac{\partial \hat{y}}{\partial z} $$ $\frac{\partial L}{\partial \hat{y}} = \frac{\partial}{\partial \hat{y}} \left( \frac{1}{2}(y - \hat{y})^2 \right) = -(y - \hat{y}) = \hat{y} - y$. $\frac{\partial \hat{y}}{\partial z} = \frac{\partial}{\partial z}(z) = 1$. So, $\frac{\partial L}{\partial z} = (\hat{y} - y) \cdot 1 = \hat{y} - y$. In our example: $\hat{y} - y = 0.5 - 0.8 = -0.3$. Now, we use $\frac{\partial L}{\partial z}$ to find the gradients with respect to $w_1, w_2,$ and $b$: $$ \frac{\partial L}{\partial w_1} = \frac{\partial L}{\partial z} \frac{\partial z}{\partial w_1} = (\hat{y} - y) \cdot x_1 = (-0.3) \cdot (1.0) = -0.3 $$ $$ \frac{\partial L}{\partial w_2} = \frac{\partial L}{\partial z} \frac{\partial z}{\partial w_2} = (\hat{y} - y) \cdot x_2 = (-0.3) \cdot (2.0) = -0.6 $$ $$ \frac{\partial L}{\partial b} = \frac{\partial L}{\partial z} \frac{\partial z}{\partial b} = (\hat{y} - y) \cdot 1 = (-0.3) \cdot 1 = -0.3 $$ The gradients are $\frac{\partial L}{\partial w_1} = -0.3$, $\frac{\partial L}{\partial w_2} = -0.6$, and $\frac{\partial L}{\partial b} = -0.3$. **3. Update Parameters: Weights and Bias** Apply the gradient descent update rule: $\theta_{\text{new}} = \theta_{\text{old}} - \eta \cdot \nabla_\theta L$, where $\theta$ is a parameter ($w_1, w_2,$ or $b$). Update $w_1$: $$ w_{1_{\text{new}}} = w_1_{\text{old}} - \eta \cdot \frac{\partial L}{\partial w_1} = 0.5 - (0.1) \cdot (-0.3) = 0.5 - (-0.03) = 0.5 + 0.03 = 0.53 $$ Update $w_2$: $$ w_{2_{\text{new}}} = w_2_{\text{old}} - \eta \cdot \frac{\partial L}{\partial w_2} = -0.1 - (0.1) \cdot (-0.6) = -0.1 - (-0.06) = -0.1 + 0.06 = -0.04 $$ Update $b$: $$ b_{\text{new}} = b_{\text{old}} - \eta \cdot \frac{\partial L}{\partial b} = 0.2 - (0.1) \cdot (-0.3) = 0.2 - (-0.03) = 0.2 + 0.03 = 0.23 $$ After one step of gradient descent with $\eta=0.1$, the updated weights are $w_{1_{\text{new}}} = 0.53$, $w_{2_{\text{new}}} = -0.04$, and the updated bias is $b_{\text{new}} = 0.23$.

# Question

Compute the derivative of a sigmoid activation function at a specific input value.

---

# Answer

The sigmoid activation function is defined as:
$$
\sigma(z) = \frac{1}{1 + e^{-z}}
$$
The derivative of the sigmoid function with respect to $z$ is given by:
$$
\sigma'(z) = \sigma(z)(1 - \sigma(z))
$$

Let's compute the derivative at a specific input value, for example, $z = 1$.

First, we need to calculate the value of the sigmoid function at $z=1$:
$$
\sigma(1) = \frac{1}{1 + e^{-1}}
$$
Using the approximation $e^{-1} \approx 0.36788$:
$$
\sigma(1) \approx \frac{1}{1 + 0.36788} = \frac{1}{1.36788} \approx 0.73106
$$

Now, substitute this value into the derivative formula $\sigma'(z) = \sigma(z)(1 - \sigma(z))$:
$$
\sigma'(1) = \sigma(1)(1 - \sigma(1))
$$
Using the calculated value $\sigma(1) \approx 0.73106$:
$$
\sigma'(1) \approx 0.73106 \times (1 - 0.73106)
$$
$$
\sigma'(1) \approx 0.73106 \times 0.26894
$$
$$
\sigma'(1) \approx 0.1966
$$

So, the derivative of the sigmoid function at $z=1$ is approximately $0.1966$.

The calculation shows that the derivative depends only on the function's output value at that point.

# Question

Consider the following for a single-layer perceptron network:

Input vector: $X=[1,0.5]$
Weights: $W=[0.6,−0.4]$
Bias: $b=0.2$

Activation function: step function.

Calculate the net input and final binary output.

---

# Answer

In a single-layer perceptron, the computation involves calculating the net input (or pre-activation) and then applying the activation function to get the final output.

Let the input vector be $x = \begin{pmatrix} 1 \\ 0.5 \end{pmatrix}$ and the weight vector be $w = \begin{pmatrix} 0.6 \\ -0.4 \end{pmatrix}$. The bias is $b=0.2$. The net input $z$ is calculated as the weighted sum of inputs plus the bias:
$$
z = w^T x + b
$$
Alternatively, using the given row vector notation for $W$ and column for $x$, the net input is:
$$
z = Wx + b
$$
Where $W$ is treated as a $1 \times 2$ matrix and $x$ as a $2 \times 1$ matrix.

**1. Calculate the Net Input ($z$)**

$$
z = (0.6)(1) + (-0.4)(0.5) + 0.2
$$
$$
z = 0.6 - 0.2 + 0.2
$$
$$
z = 0.6
$$
The net input to the neuron is $0.6$.

**2. Calculate the Final Binary Output**

The activation function is the step function. A common definition for the step function is:
$$
f(z) = \begin{cases} 1 & \text{if } z \ge 0 \\ 0 & \text{if } z < 0 \end{cases}
$$
We apply this function to the net input $z = 0.6$.
Since $z = 0.6$ is greater than or equal to 0 ($0.6 \ge 0$), the step function outputs 1.
$$
\text{Output} = f(0.6) = 1
$$

The final binary output of the perceptron for the given input is $1$.