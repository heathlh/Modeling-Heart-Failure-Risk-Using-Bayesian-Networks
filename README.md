# 🩺 Modeling Heart Failure Risk Using Bayesian Networks

[cite\_start]A Transparent and Probabilistic Approach to Clinical Decision Support (ADSP 32014 IP01: Bayesian Machine Learning) [cite: 2, 4]

## Project Overview

[cite\_start]This project explores the use of **Bayesian Networks (BN)** for heart disease risk prediction using the Heart Failure Prediction Dataset (Kaggle)[cite: 25]. [cite\_start]In high-stakes domains like clinical risk assessment, **interpretability** and **uncertainty quantification** are critical, yet often lacking in traditional "black-box" machine learning models[cite: 10, 14].

[cite\_start]Our primary goal was to develop a **transparent, probabilistic model** that maintains competitive predictive performance while offering fully calibrated, evidence-based reasoning[cite: 11, 112].

### Key Findings

  * [cite\_start]**High Predictive Power:** the Bayesian Network achieved an **ROC-AUC of 0.916** [cite: 50][cite\_start], nearly matching the top-performing discriminative model (Logistic Regression, AUC 0.930)[cite: 48, 50, 74, 78].
  * [cite\_start]**Robust Calibration:** the BN demonstrated superior probability **calibration** [cite: 55, 56] (Brier Score 0.125) [cite\_start][cite: 76, 79][cite\_start], providing highly reliable uncertainty estimates essential for clinical use[cite: 54, 80].
  * [cite\_start]**White-Box Interpretability:** the model supports transparent, scenario-based probabilistic queries, allowing clinicians to understand *why* a specific risk was assigned[cite: 20, 83, 114].

-----

## 💻 Methodology

### 1\. Data Preprocessing

[cite\_start]The model was trained on **918 real-world patient records** [cite: 30] [cite\_start]from the Heart Failure Prediction Dataset[cite: 25].

[cite\_start]To align with Bayesian Network requirements and clinical standards, continuous numerical features were converted into **clinically meaningful, interpretable bins** (discretization)[cite: 27, 27].

| Feature | [cite\_start]Example Discretization Bins [cite: 28] |
| :--- | :--- |
| **Age** | [cite\_start]{\<40, 40-55, 55-70, $\ge$70} [cite: 32] |
| **RestingBP** | [cite\_start]{\<120, 120-140, 140-160, $\ge$160} [cite: 33] |
| **Cholesterol** | [cite\_start]{\<200, 200-240, $\ge$240} [cite: 34] |

### 2\. Bayesian Network (BN) Modeling

[cite\_start]The BN was constructed using the `bnlearn` and `pgmpy` libraries[cite: 39].

  * [cite\_start]**Structure Learning:** We used **Hill-Climbing** with the **Bayesian Information Criterion (BIC)** score to learn the Directed Acyclic Graph (DAG) structure that best explains the dependencies between risk factors[cite: 39].
  * [cite\_start]**Parameter Learning:** **Bayesian Estimation** with Dirichlet priors was used to estimate the Conditional Probability Distributions (CPDs) for each node[cite: 39, 21].

[cite\_start]The resulting structure explicitly models conditional dependencies between clinical factors[cite: 15, 16]:

[cite\_start]*The BN structure reveals key relationships, such as the strong influence of the **ST\_Slope** ECG result on the **HeartDisease** diagnosis[cite: 40].*

### 3\. Probabilistic Inference & Prediction

The BN was evaluated by computing the posterior probability:

[cite\_start]$$P(\text{HeartDisease} = 1 \mid \text{evidence})$$ [cite: 19, 86, 90]

[cite\_start]for each patient in the test set using **Variable Elimination**[cite: 43].

-----

## 📈 Results and Evaluation

### [cite\_start]Model Performance Metrics [cite: 60]

The BN proved highly competitive, particularly in terms of probabilistic reliability.

| [cite\_start]Model [cite: 61] | [cite\_start]ROC-AUC (Discrimination) [cite: 62] | [cite\_start]Accuracy [cite: 63] | [cite\_start]Brier Score (Calibration Error) [cite: 64] |
| :--- | :--- | :--- | :--- |
| [cite\_start]**Logistic Regression** [cite: 65] | [cite\_start]0.930 [cite: 66] | [cite\_start]0.886 [cite: 67] | [cite\_start]N/A [cite: 68] |
| [cite\_start]**Decision Tree (Max Depth 4)** [cite: 69] | [cite\_start]0.870 [cite: 70] | [cite\_start]0.804 [cite: 71] | [cite\_start]N/A [cite: 72] |
| [cite\_start]**Bayesian Network** [cite: 73] | [cite\_start]0.916 [cite: 74] | [cite\_start]N/A [cite: 75] | [cite\_start]**0.125** [cite: 76] |

[cite\_start]**Conclusion:** The BN (0.916 AUC) is robust and maintains high discrimination[cite: 78]. [cite\_start]The low Brier Score (0.125) confirms the BN's probabilities are highly reliable[cite: 79]. [cite\_start]The BN offers the best blend of predictive power and statistical reliability/interpretability[cite: 80].

### [cite\_start]Discrimination (ROC Curve) [cite: 44]

[cite\_start]The ROC curve shows the BN's (green line) ability to rank patients by risk is significantly better than the Decision Tree and close to the Logistic Regression model[cite: 46].

### [cite\_start]Calibration (Calibration Curve) [cite: 53]

[cite\_start]The calibration curve demonstrates the BN's superior ability to provide reliable probability estimates[cite: 55]. [cite\_start]The BN (green line) tracks the **"Perfect"** line (dashed black) more closely than the Decision Tree (orange) and exhibits highly stable calibration[cite: 56].

### [cite\_start]Interpretable Clinical Queries [cite: 82]

[cite\_start]The BN's structure allows for transparent, scenario-based inference, directly supporting clinician trust[cite: 83].

  * [cite\_start]**Scenario 1: Elderly with Symptoms**[cite: 84]:

      * [cite\_start]Evidence: $\text{Age} \ge 70$ AND $\text{Exercise Angina} = Y$ [cite: 85]
      * [cite\_start]$P(\text{HeartDisease} = 1 \mid \text{Evidence}) = 0.588$ [cite: 86]
      * [cite\_start]*(Query shows a high likelihood of disease given the compounded risk factors.)* [cite: 87]

  * [cite\_start]**Scenario 2: Young, Healthy ECG**[cite: 88]:

      * [cite\_start]Evidence: $\text{Age} < 40$ AND $\text{ST\_Slope} = \text{Up}$ AND $\text{Oldpeak} \le 0$ [cite: 89]
      * [cite\_start]$P(\text{HeartDisease} = 1 \mid \text{Evidence}) = 0.265$ [cite: 90]
      * [cite\_start]*(Protective factors significantly lower the estimated risk, providing transparent justification.)* [cite: 91]

-----

## ✨ Generative AI Application: Synthetic Data

[cite\_start]As a generative model, the BN can simulate realistic patient data, a key application in ADSP[cite: 93, 94].

### [cite\_start]Conditional Sampling [cite: 95]

[cite\_start]We generated 1,000 synthetic patient profiles conditioned on evidence (e.g., $\text{Age} \ge 70$) to model specific patient cohorts[cite: 96].

  * **Code Snippet:**
    ````python
    sampler = BayesianModelSampling(bn)
    synthetic_data = sampler.rejection_sample(
        evidence=[('Age_bin', '>=70')],
        size=1000
    )
    # Estimated P(HeartDisease=1 | Age_bin>=70) from samples ≈ 0.502
    [cite_start]``` [cite: 97, 98, 99, 100, 101, 104]

    ````
  * [cite\_start]**Validation:** The empirical risk (47.6%) derived from the samples aligned with the analytical inference results[cite: 104, 107].

### Utility

  * [cite\_start]**Research Utility:** Augmenting small datasets for rare subgroups[cite: 105].
  * [cite\_start]**Privacy Utility:** Sharing statistical properties of patient data without exposing real PHI (Protected Health Information)[cite: 106].

-----

## 🚀 Conclusion

[cite\_start]The Bayesian Network is the optimal model for this high-stakes task[cite: 112]. It provides an unparalleled combination of:

1.  [cite\_start]**Performance** (AUC 0.916, Brier 0.125) [cite: 113]
2.  [cite\_start]**Interpretability** (Supports exact probabilistic queries) [cite: 114]
3.  [cite\_start]**Generative AI** (Demonstrates utility in synthetic data generation) [cite: 115]
