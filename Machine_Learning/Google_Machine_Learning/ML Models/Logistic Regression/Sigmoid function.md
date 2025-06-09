Many problems require a probability estimate as output. [**Logistic regression**](https://developers.google.com/machine-learning/glossary#logistic_regression) is an extremely efficient mechanism for calculating probabilities. Practically speaking, you can use the returned probability in either of the following two ways:

- Applied "as is." For example, if a spam-prediction model takes an email as input and outputs a value of `0.932`, this implies a `93.2%` probability that the email is spam.
- Converted to a [**binary category**](https://developers.google.com/machine-learning/glossary#binary-classification) such as `True` or `False`, `Spam` or `Not Spam`.

This module focuses on using logistic regression model output as-is. In the [Classification module](https://developers.google.com/machine-learning/crash-course/classification/index.md), you'll learn how to convert this output into a binary category.