## 🧪 Statistical Significance & P-Values
![[Pasted image 20250609103847.png]]

### 🎯 Goal
To decide whether the **observed outcome** (e.g. coin flips) is surprising **enough** under the assumption that the **null hypothesis** is true.

### 🧠 Core Concepts
#### Null Hypothesis (H₀)
> "This is a fair coin."  
> We assume this is true by default.
#### Alternative Hypothesis (H₁)
> "This is not a fair coin."  
> We seek evidence to reject H₀ in favor of H₁.

### 🔄 Coin Toss Example
We toss a coin **six times** and observe:
```
Tails, Tails, Tails, Tails, Tails, Tails
```

Each toss under H₀ (fair coin) has a 50% chance of tails.

|Number of Tosses|Probability of All Tails (under H₀)|Feeling|
|---|---|---|
|1|0.5|Normal|
|2|0.25|Still fine|
|3|0.125|Mild suspicion|
|4|0.0625|Uneasy|
|5|0.03125|Very uneasy|
|6|0.015625|Highly suspicious|

### 📉 P-Value
- **Definition**: The probability of observing a result at least as extreme as the one seen, **assuming H₀ is true**.
- As you observe more tails, **P-value drops**.
- The **lower the P-value**, the **less likely** this outcome is under H₀.

### 📏 Statistical Significance
- We choose a threshold called **alpha (α)**, often set at **0.05 (5%)**.
- If **P-value ≤ α**, we **reject H₀** and say the result is **statistically significant**.
- In this case, 6 tails in a row → P ≈ 0.01 → significant if α = 0.05

### 💡 Intuition

> Statistical significance formalizes your **"uneasy feeling"** about H₀ using math.  
> When something feels "too rare to be random", the P-value gives you the number to prove it.
### ⚙️ Customizing Alpha

| Use Case                    | Alpha (α) Level |
| --------------------------- | --------------- |
| General purpose             | 0.05            |
| Risk-tolerant               | 0.10            |
| High-stakes (e.g. medicine) | 0.01 or lower   |
