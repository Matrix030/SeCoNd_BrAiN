![[Pasted image 20250609094216.png]]

## For categorical variable -
Approach - Create Dummy Variables ![[Pasted image 20250609094406.png]]
### 🧩 What Are Dummy Variables?

A **dummy variable** is a binary (0/1) column used to represent categorical values numerically.

- Since "State" has 2 values (New York and California), we can create **1 dummy variable per category**, but we **only use (k – 1)** dummy variables to avoid **Multicollinearity** (a trap called the _Dummy Variable Trap_). ^ef3e1a

Here, we represent "State" using two dummies:
- **New York → 1 or 0**
- **California → 1 or 0**

But in practice, only one of them needs to be included in the equation. The other can be inferred.

## Dummy Variable Trap
### 🧠 What Is Multicollinearity?

In multiple linear regression, **multicollinearity** means **two or more input variables are so strongly correlated** that the model struggles to tell them apart.

Think of it like having two students giving almost identical answers on a test. You can’t tell who knows what—they’re redundant.

When this happens in regression, the model’s math (matrix inversion) becomes unstable. Coefficients can become wildly inaccurate, or the model might fail to compute them at all.
### 📦 Now Add Categorical Data (Like State)

Suppose you have a categorical variable like `"State"` with three categories:
- **New York**
- **California**
- **Florida**

To use these in a regression model, we convert them into dummy variables:

| State      | New York | California | Florida |
| ---------- | -------- | ---------- | ------- |
| New York   | 1        | 0          | 0       |
| California | 0        | 1          | 0       |
| Florida    | 0        | 0          | 1       |

Now here’s the trap:
### ❌ Dummy Variable Trap = Perfect Multicollinearity

If you include **all three dummy variables** in your model **along with an intercept**, you create a perfect linear dependency:

New York+California+Florida=1

This is always true. So if the model knows two of these, it can always figure out the third—there’s no new information.

That’s a textbook case of **perfect multicollinearity**.

### ✅ How to Avoid It: Drop One Dummy

Instead of using all three, drop one:
- Use only two dummy variables.
- Let the **dropped category be the reference** (or baseline).
- The dropped category’s effect is absorbed into the **intercept term** b0.

Now:
- Florida is the baseline.
- New York and California are compared _to Florida_.

This removes redundancy and keeps the model stable.