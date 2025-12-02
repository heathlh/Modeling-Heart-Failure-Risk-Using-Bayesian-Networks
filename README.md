# **Modeling Heart Failure Risk Using Bayesian Networks**

*A Transparent and Probabilistic Approach to Clinical Decision Support*

---

## **Overview**

This project applies **Bayesian Networks (BNs)** to heart disease risk prediction using the **Heart Failure Prediction Dataset**. In clinical settings—where decisions are high-stakes—BNs offer **interpretability**, **transparent reasoning**, and **uncertainty quantification**, which traditional black-box models often lack.

### **Key Findings**

* **High Predictive Power**
  BN achieves **ROC-AUC: 0.916**, nearly matching Logistic Regression (0.930).

* **Superior Calibration**
  Brier Score **0.125**, indicating highly reliable probability estimates.

* **White-Box Interpretability**
  The BN supports conditional queries and scenario reasoning, explaining *why* risks are assigned.

---

## **Methodology**

### **1. Data Preprocessing**

* Dataset contains **918 patient records**.
* Continuous features are discretized into **clinically interpretable bins**.

| Feature         | Example Bins                 |
| --------------- | ---------------------------- |
| **Age**         | <40, 40–55, 55–70, ≥70       |
| **RestingBP**   | <120, 120–140, 140–160, ≥160 |
| **Cholesterol** | <200, 200–240, ≥240          |

---

### **2. Bayesian Network Modeling**

Built using **bnlearn** and **pgmpy**:

* **Structure Learning:** Hill-Climbing + BIC
* **Parameter Learning:** Bayesian estimation with Dirichlet priors
* The final DAG captures clinically meaningful dependencies (e.g., **ST_Slope → HeartDisease**).

---

### **3. Probabilistic Inference**

For each patient, the BN computes:

[
P(HeartDisease=1 \mid \evidence)
]

Inference performed via **Variable Elimination**.

---

## **Results**

### **Model Comparison**

| Model                   | ROC-AUC   | Accuracy | Brier Score |
| ----------------------- | --------- | -------- | ----------- |
| Logistic Regression     | **0.930** | 0.886    | —           |
| Decision Tree (Depth=4) | 0.870     | 0.804    | —           |
| **Bayesian Network**    | **0.916** | —        | **0.125**   |

**BN = Best balance of interpretability + calibrated probabilistic performance**

---

### **ROC Curve**

BN discrimination is close to Logistic Regression and better than Decision Trees.

### **Calibration Curve**

BN probabilities follow the perfect-calibration line more closely than other models.

---

## **Interpretable Clinical Queries**

### **Scenario 1 — High-Risk Case**

* **Evidence:** Age ≥ 70 AND Exercise Angina = Yes
* **Result:** (P(HeartDisease=1) = 0.588)
* **Interpretation:** Combined risk factors significantly elevate risk.

---

### **Scenario 2 — Low-Risk Case**

* **Evidence:** Age < 40 AND ST_Slope = Up AND Oldpeak ≤ 0
* **Result:** (P(HeartDisease=1) = 0.265)
* **Interpretation:** Protective ECG features reduce estimated risk.

---

## **Generative AI Application**

### **Synthetic Data Generation**

Bayesian Networks also support conditional sampling:

```python
sampler = BayesianModelSampling(bn)
synthetic_data = sampler.rejection_sample(
    evidence=[('Age_bin', '>=70')],
    size=1000
)
```

* Synthetic cohort aligns with analytical inference
* Useful for:

  * Augmenting rare subgroups
  * Privacy-preserving data sharing
  * Scenario planning

---

## **Conclusion**

The Bayesian Network provides:

✔ **Strong Performance** – AUC 0.916, Brier 0.125

✔ **Full Interpretability** – Clinician-friendly reasoning

✔ **Generative Power** – Synthetic data for simulation & analysis

BNs are a **transparent, reliable, and clinically aligned** alternative to black-box models for medical decision support.

---
