---
tags: [ml, udemy, regression, linear-regression, multiple-linear-regression]
---
### 🧠 What Is It?
> This method evaluates **every possible combination of predictors** and chooses the **best model** based on a performance metric (e.g., **Adjusted R²**, **AIC**, **BIC**).
### 🧪 How It Works
#### **Step 1**: Choose a metric for model quality
- Examples:
    - Adjusted R²
    - AIC (Akaike Information Criterion)
    - BIC (Bayesian Information Criterion)

#### **Step 2**: Generate all possible models
- For `n` variables → total models = `2ⁿ - 1`
    - Example: 10 variables → **1,023 models**
    - 100 variables → **~1.27×10³⁰ models** (infeasible)
#### **Step 3**: Evaluate and select the best model
- Pick the one with the best score based on your chosen metric.
### ⚠️ Drawbacks
- **Computationally expensive**
- **Explodes exponentially** with more variables
- Often impractical without **advanced computing power**