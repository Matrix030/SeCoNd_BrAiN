# CART
	1.Classification Trees
	2.Regression Trees

## Regression Trees
![[Pasted image 20250610102554.png]]
Scatter plot of a dataset
2 independent variable (x1, x2)

- y (depended) - to be predicted - third dimension (sticking out of your screen)
![[Pasted image 20250610102715.png]]

When the alogrithm is run a few splits happen
![[Pasted image 20250610102857.png]]

How are they conducted are did by the algorithm (information entropy)

is the split increasing amount of information, does it add value?


## Creating Decision Tree
![[Pasted image 20250610103146.png]]
Step 1:
![[Pasted image 20250610103207.png]]


![[Pasted image 20250610103220.png]]
Step 2:
![[Pasted image 20250610103234.png]]
![[Pasted image 20250610103243.png]]
Step 3:
![[Pasted image 20250610103255.png]]
![[Pasted image 20250610103259.png]]
Step 4:
![[Pasted image 20250610103316.png]]

Once everything is plotted we predict Y
by adding splits we add information.

## 🌳 Decision Trees: Regression Trees (CART)
### 📘 What is CART?
- **CART** = _Classification and Regression Trees_
- Umbrella term for:
    - **Classification Trees** (predict categories)
    - **Regression Trees** (predict numerical values)
> This section focuses on **Regression Trees**, which are more complex but powerful for predicting continuous variables.

### 🎯 Goal
To predict a **continuous output** y, using one or more **input features** (e.g. x1,x2x_1, x_2x1​,x2​) by segmenting the input space and assigning a prediction to each segment.
### 🧱 Dataset Structure
- Input: Two features → x1​, x2​
- Target: One dependent variable → y
- Visualized as a **scatterplot in 2D** (x1 vs x2), with y being the third dimension we don’t see

### ✂️ How Regression Trees Work
#### Step-by-step:
1. **Start with all data points**  
    (You don’t need to see the output yyy yet — just the feature space)
2. **Split the feature space** based on thresholds in x1x_1x1​, x2x_2x2​
    - Example splits:
        - x1<20x1 < 20x1​<20
        - x2<170x_2 < 170x2​<170
        - x2<200x_2 < 200x2​<200
        - x1<40x_1 < 40x1​<40
3. **Continue splitting** until:
    - No significant gain in information (via entropy or variance reduction)
    - Minimum leaf size is reached
    - Max depth or other stopping criteria are met
4. **Each terminal leaf** represents a **segment** of the input space
    
### 🧮 What’s in Each Leaf?
Each terminal leaf stores the **average of y-values** for the data points that fall into it.
So:
- Any **new data point** passed to the tree is routed to a **leaf**
- That leaf returns the **average y** as the prediction
### 🌐 Conceptual Advantage
> Instead of using one global average of yyy, the tree breaks the space into **regions with local averages**.  
> This increases **prediction accuracy** by leveraging the structure in the input space.

### 📉 Default vs. Tree-based Prediction

|Method|Output Strategy|
|---|---|
|No model|Always return **mean(y)**|
|Regression Tree|Return **mean(y)** from the matched **leaf**|