### 🧠 What Is It?
> **Bidirectional Elimination** combines the ideas of **Forward Selection** and **Backward Elimination**.  
> You both **add** variables and **remove** them in each iteration based on their **P-values**.

Often referred to as **stepwise regression**, this is a more **dynamic**, iterative approach.

### 🔁 Step-by-Step Procedure
#### **Step 1**: Set two significance levels
- `SL_enter` = significance level to **enter** the model (e.g., 0.05)
- `SL_stay` = significance level to **stay** in the model (e.g., 0.05)
#### **Step 2**: Forward Selection step
- Add the variable with the **lowest P-value** that satisfies `P < SL_enter`.
#### **Step 3**: Backward Elimination step
- Check **all existing variables**.
- Remove any variable where `P > SL_stay`.    
- You can remove **one or more** variables in this step.

🔁 **Repeat Steps 2 and 3**:
- Keep growing the model by adding new variables.
- Keep pruning it by removing insignificant ones.

### 🛑 Stop When:
- No more variables can be added (**none meet `SL_enter`**), **and**
- No more variables can be removed (**all meet `SL_stay`**)