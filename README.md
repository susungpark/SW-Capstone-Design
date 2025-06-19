# 25 -1 SW-Capstone-Design

## Reflections on improving portfolio downside risk through financial statement-based clustering methodologies

## 📌 Project Overview

This project aims to evaluate the performance of **financial-statement-based clustering** in constructing stock portfolios. Using the portfolio optimization method and performance evaluation criteria, we empirically examine whether machine learning-based clusters outperform in portfolio evaluation metrics, compared to traditional groupings.

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

  The Clustering method is constructed based our major purpose, which is comparing the portfolios of random selected portfolios vs. financial-based clustered portfolios.

  The windows were evaluated seperately, each containing 2 years of in-sample and 1 year of out-sample.


### 2. Portfolio Construction

* **Inter-Cluster Sampling**:
  One stock is sampled from each cluster to construct a portfolio.
  Repeated 1,000 times to ensure statistical robustness.

* **Random Sampling**
  One stock is sampled matching the size of the Inter-Cluster Portfolio.
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
\text{CVaR}_\alpha = \mathbb{E}\left[R \mid R \leq \text{VaR}_\alpha\right]
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

#### For Random vs. Financial Statement

* **In-Sample**: 2020–2021, 2021-2022, 2022-2023 (2yrs per window)
* **Out-of-Sample**: 2022, 2023, 2024 (1yr per window)


---

## 📊 Result Structure

* Results are saved under:
  - `results/{window}/ttest_{window}_{method}_{sample_type}.csv`
  - `results/gics_vs_kmeans/ttest_gics_vs_kmeans_{method}_{sample_type}.csv`

* Each CSV file includes:

  * Mean ± Standard Deviation of performance metrics
  * One-sided t-test statistics comparing KMeans > GICS
  * Significance level indicators (`<0.1 : *, <0.05: **, <0.01: ***`)

---

## 🔁 Workflow Summary

1. Perform KMeans clustering on financial features.
2. Construct 1,000 inter-cluster portfolios for both Random and Financial Statement based Clusters.
3. Optimize portfolios using Markowitz with a minimum variance constraint.
4. Evaluate each portfolio’s in-sample and out-of-sample performance.
5. Conduct statistical tests to compare methods.

---

## 🧪 Technologies & Libraries

* Python (pandas, numpy, scikit-learn, scipy, statsmodels, matplotlib, seaborn)
* Optimization via `scipy.optimize.minimize`
* Evaluation & visualization: `SHAP`, `LIME`, `tsne`, `PCA`

---

## 💻 How to Run the Code

1. Clone this repository
2. Install required libraries:
  -> Check "Technologies and Libraries Section"


### ✅ Demo (Sample Results)**  
- Section:
  - `## 📊 Result Structure`  
  - The actual results are saved in the code/results folder


### 📁 Example Output

| Metric | KMeans Mean±Std   | GICS Mean±Std    | t-stat | p-value | Signif. |
|--------|-------------------|------------------|--------|---------|---------|
| CVaR   | 0.4213 ± 0.0234   | 0.3841 ± 0.0218  | 2.114  | 0.0184  | **      |

> **Note**: The example output may differ from the actual results.


---

## 👤 Author

* **Name**: Taekyoung Lee , Sungsu Park
* **Affiliation**: Kyung Hee University

  * Department of Industrial & Management Engineering, Software Convergence
  * School of Engineering, Software Convergence

* **Research Focus**:
  Machine Learning for Asset Selection, Portfolio Optimization, Risk Management, Financial Engineering, Clusterting, Explainable AI
