## Anscombe's Quartet
![[Pasted image 20250609092342.png]]

You  can't just blindly apply Linear Regression. you have to make sure that your dataset is fit for Linear Regressions.

### There are 5 Assumptions + an Extra Check:
1. Linearity - Make sure that there is a Linear relationship between the independent and the dependent variable.![[Pasted image 20250609092551.png]]
2. Homoscedasticity - Equal Variance - You don't want to see a cone shape in your data points.![[Pasted image 20250609092656.png]]

3. Multivariate Normality - Normality of error distribution - If you look along the line of your linear regression you **want** see a normal distribution of your data points.![[Pasted image 20250609092836.png]]
4. Independence of observations (Included "no autocorrelation" - we don't want to see any kind of pattern in our data, pattern indicates that our rows are not independent, i.e some rows are affecting other rows etc. Eg. stock market - previous prices affect future prices.) - Therefore, no Linear Regression model![[Pasted image 20250609093125.png]]
5. Lack of [[Dummy Variables#🧠 What Is Multicollinearity?| Multicollinearity]] - Predictors are not correlated with each other. Example - [[Dummy Variables#Dummy Variable Trap | Dummy Variable Trap]]       ![[Pasted image 20250609093248.png]]
6. The Outlier Check - Extra Check - sometimes outliers significantly affect the Linear Regression.![[Pasted image 20250609093352.png]]
