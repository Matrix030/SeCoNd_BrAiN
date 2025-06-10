# Multiple Linear Regression Equation

Let:
- `X₁`: R&D Spend  
- `X₂`: Administration  
- `X₃`: Marketing Spend  
- `X₄`, `X₅`: One-hot encoded State variables (e.g., California, Florida – assuming New York is the base case)

Then the regression model is:

```
Y = β₀ + β₁·X₁ + β₂·X₂ + β₃·X₃ + β₄·X₄ + β₅·X₅ + ε
```

Where:
- `Y` is the predicted **Profit**
- `β₀` is the **intercept**
- `β₁` to `β₅` are the **coefficients** or **weights**
- `ε` is the **error term**

![[Pasted image 20250609091824.png]]


