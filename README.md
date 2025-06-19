# 25 -1 SW-Capstone-Design

## Reflections on improving portfolio downside risk through financial statement-based clustering methodologies

## 📌 Project Overview

This project aims to compare the performance of **financial-statement-based clustering** and **GICS-based classification** in constructing stock portfolios. Using the same optimization method and performance evaluation criteria, we empirically examine whether machine learning-based clusters outperform traditional industry-based groupings.

---

## 🗂️ Data Description

### Input Files

* `financial_data_processing_cospi200.csv`
  Processed financial statement data of KOSPI 200 firms.

* `종목_GICS분류_클러스터.csv`
  GICS classification information mapped to the same firms.

* `log_returns_total.csv`
  Daily log returns over 5 years (2020–2024).

---

## 🔧 Methodology

### 1. Clustering Approaches

* **GICS-Based**:
  Uses sector-level GICS clusters directly.

* **Financial-Based (KMeans)**:
  Applies KMeans clustering with the same number of clusters as in the GICS dataset.

### 2. Portfolio Construction

* **Inter-Cluster Sampling**:
  One stock is sampled from each cluster to construct a portfolio.
  Repeated 1,000 times to ensure statistical robustness.

### 3. Portfolio Optimization

* **Minimum Variance Portfolio with Target Return Constraint**
  Target returns range from 0.5% to 5.0% (in 0.5% increments).

#### 📐 Objective Function (Minimum Variance Portfolio)

$$
\min_{w} \ w^T \Sigma w \\
\text{subject to:} \\
\sum_{i=1}^n w_i = 1 \\
w^T \mu \geq r_\text{target}
$$

Where:

* \$w\$: portfolio weights
* \$\Sigma\$: covariance matrix of asset returns
* \$\mu\$: vector of expected returns
* \$r\_\text{target}\$: minimum target return

### 4. Performance Metrics

#### 📊 Evaluation Criteria

* **Sortino Ratio**:

  $$
  \text{Sortino} = \frac{\mathbb{E}[R - R_f]}{\sigma_\text{downside}}
  $$

* **Conditional Value at Risk (CVaR)**:

  $$
  \text{CVaR}_\alpha = \mathbb{E}[R | R \leq \text{VaR}_\alpha]
  $$

* **Maximum Drawdown (MDD)**:

  $$
  \text{MDD} = \min_t \left( \frac{V_t - \max_{s \leq t} V_s}{\max_{s \leq t} V_s} \right)
  $$

* **Omega Ratio**:

  $$
  \Omega(r) = \frac{\int_r^\infty (1 - F(x)) dx}{\int_{-\infty}^r F(x) dx}
  $$

### 5. Evaluation Periods

* **In-Sample**: 2020–2022
* **Out-of-Sample**: 2023–2024

---

## 📊 Result Structure

* Results are saved under:
  `results/gics_vs_kmeans/ttest_gics_vs_kmeans_{method}_{sample_type}.csv`

* Each CSV file includes:

  * Mean ± Standard Deviation of performance metrics
  * One-sided t-test statistics comparing KMeans > GICS
  * Significance level indicators (`*, **, ***`)

---

## 🔁 Workflow Summary

1. Perform KMeans clustering on financial features.
2. Construct 1,000 inter-cluster portfolios for both GICS and KMeans clusters.
3. Optimize portfolios using Markowitz with a minimum variance constraint.
4. Evaluate each portfolio’s in-sample and out-of-sample performance.
5. Conduct statistical tests to compare methods.

---

## 🧪 Technologies & Libraries

* Python (pandas, numpy, scikit-learn, scipy, statsmodels, matplotlib, seaborn)
* Optimization via `scipy.optimize.minimize`
* Evaluation & visualization: `SHAP`, `LIME`, `tsne`, `PCA`

---

## ✅ Future Extensions

* Try alternative clustering methods (e.g., GMM, Spectral Clustering)
* Optimize number of clusters using DB Index, Silhouette Score
* Add constraints (e.g., minimum number of stocks per cluster)
* Use Explainable AI for cluster interpretation

---

## 👤 Author

* **Name**: Taekyoung Lee (이태경)
* **Affiliation**: Kyung Hee University

  * Department of Industrial & Management Engineering
  * School of Software Convergence
* **Research Focus**:
  Machine Learning for Asset Selection, Clustering-Based Portfolio Optimization, Financial Engineering
