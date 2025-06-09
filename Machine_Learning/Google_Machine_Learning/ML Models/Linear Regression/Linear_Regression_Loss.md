[Linear regression: Loss  |  Machine Learning  |  Google for Developers](https://developers.google.com/machine-learning/crash-course/linear-regression/loss)

# Linear regression: Loss

## (IMP) - Note the relationship between the model and the data:

- **MSE**. The model is closer to the outliers but further away from most of the other data points.
    
- **MAE**. The model is further away from the outliers but closer to most of the other data points.

## Loss
[**Loss**](https://developers.google.com/machine-learning/glossary#loss) is a numerical metric that describes how wrong a model's [**predictions**](https://developers.google.com/machine-learning/glossary#prediction) are. Loss measures the distance between the model's predictions and the actual labels. The goal of training a model is to minimize the loss, reducing it to its lowest possible value.

In the following image, you can visualize loss as arrows drawn from the data points to the model. The arrows show how far the model's predictions are from the actual values.

In statistics and machine learning, loss measures the difference between the predicted and actual values. Loss focuses on the _distance_ between the values, not the direction.

The two most common methods to remove the sign are the following:

- Take the absolute value of the difference between the actual value and the prediction.
- Square the difference between the actual value and the prediction.
## Types of loss

In linear regression, there are four main types of loss, which are outlined in the following table.
![[Pasted image 20250608125229.png]]
The functional difference between L1 loss and L2 loss (or between MAE and MSE) is squaring. When the difference between the prediction and label is large, squaring makes the loss even larger. When the difference is small (less than 1), squaring makes the loss even smaller.

When processing multiple examples at once, we recommend averaging the losses across all the examples, whether using MAE or MSE.

## Choosing a loss

Deciding whether to use MAE or MSE can depend on the dataset and the way you want to handle certain predictions. Most feature values in a dataset typically fall within a distinct range. For example, cars are normally between 2000 and 5000 pounds and get between 8 to 50 miles per gallon. An 8,000-pound car, or a car that gets 100 miles per gallon, is outside the typical range and would be considered an [**outlier**](https://developers.google.com/machine-learning/glossary#outliers).

An outlier can also refer to how far off a model's predictions are from the real values. For instance, 3,000 pounds is within the typical car-weight range, and 40 miles per gallon is within the typical fuel-efficiency range. However, a 3,000-pound car that gets 40 miles per gallon would be an outlier in terms of the model's prediction because the model would predict that a 3,000-pound car would get between 18 and 20 miles per gallon.

When choosing the best loss function, consider how you want the model to treat outliers. For instance, MSE moves the model more toward the outliers, while MAE doesn't. L2 loss incurs a much higher penalty for an outlier than L1 loss. For example, the following images show a model trained using MAE and a model trained using MSE. The red line represents a fully trained model that will be used to make predictions. The outliers are closer to the model trained with MSE than to the model trained with MAE.
![[Pasted image 20250608125302.png]]
![[Pasted image 20250608125312.png]]
