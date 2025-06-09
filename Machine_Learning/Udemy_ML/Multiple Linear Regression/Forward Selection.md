#### 🧠 What Is It?
 **Forward Selection** is a stepwise model-building method where you **start with nothing** and **add one variable at a time**, choosing the most statistically significant one at each step.
It’s the opposite of Backward Elimination, but **not just reversed**—it involves **constructing and comparing many models** as you build up.

#### 🔁 Step-by-Step Procedure
##### **Step 1**: Set a Significance Level (SL)
- Example: `SL = 0.05` (5%)
##### **Step 2**: Fit all simple linear models
- Run `y ~ x₁`, `y ~ x₂`, ..., `y ~ xₙ` (each with **one variable**)
- Choose the one with the **lowest P-value**
    - If `P < SL`, keep this variable    
##### **Step 3**: Add one more variable
- Fit all possible models that include the chosen variable **plus** one new predictor.
- Again, choose the model where the **new** variable has the **lowest P-value**
##### **Step 4**: Check significance
- If the new variable’s `P < SL`, **keep it** and repeat from Step 3
- If not, **STOP** and go to **FIN**
##### **FIN**: Keep the previous model
- You stop **before** adding a non-significant variable
- The final model includes **only significant predictors**

#### ⚙️ How It Works Visually
At each step, you're:
1. Growing the model by one variable
2. Re-evaluating the significance of the new addition
3. Keeping track of the best-performing variables

🔁 Repeat until no new variable meets the P-value threshold.