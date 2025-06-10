![[Pasted image 20250609111421.png]]
#### 🧠 What Is It?
> **Backward Elimination** is a step-by-step model simplification technique.  
> We start with **all predictors** and gradually remove the **least significant** ones based on **P-values**, until only the most relevant predictors remain.

#### 📋 Why Use It?
- Ideal when you have **many variables**, but only a few are truly important.
- Helps build a **clean, interpret-able model**.
- Based on **statistical significance** of variables.
#### 🧪 Step-by-Step Procedure
##### **Step 1**: Set the significance level (SL)
- Common choice: `SL = 0.05`
##### **Step 2**: Fit the **full model**
- Include **all predictors** (this is like the “all-in” method).
##### **Step 3**: Find the predictor with the **highest P-value**
- If **P > SL**, continue to step 4.
- Otherwise, go to **FINISH** — the model is finalized.
##### **Step 4**: Remove that predictor
- This variable is statistically insignificant.
##### **Step 5**: Refit the model
- Rebuild the regression with the remaining variables.
- Coefficients will **change** after removing a variable.

🔁 **Repeat** steps 3–5 until **all remaining predictors** have P-values < SL.
#### ✅ When Do You Stop?

> When **no predictor’s P-value** exceeds the significance level (e.g. 0.05).  
> At this point, the model is considered **optimized** and **statistically valid**.
