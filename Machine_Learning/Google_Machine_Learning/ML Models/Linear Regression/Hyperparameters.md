[Linear regression: Hyperparameters  |  Machine Learning  |  Google for Developers](https://developers.google.com/machine-learning/crash-course/linear-regression/hyperparameters)
# Linear regression: Hyperparameters
[**Hyperparameters**](https://developers.google.com/machine-learning/glossary#hyperparameter) are variables that control different aspects of training. Three common hyperparameters are:

- [**Learning rate**](https://developers.google.com/machine-learning/glossary#learning-rate)
- [**Batch size**](https://developers.google.com/machine-learning/glossary#batch-size)
- [**Epochs**](https://developers.google.com/machine-learning/glossary#epoch)
In contrast, [**parameters**](https://developers.google.com/machine-learning/glossary#parameter) are the variables, like the weights and bias, that are part of the model itself. In other words, hyperparameters are values that you control; parameters are values that the model calculates during training.

## Learning rate

[**Learning rate**](https://developers.google.com/machine-learning/glossary#learning-rate) is a floating point number you set that influences how quickly the model converges. **If the learning rate is too low, the model can take a long time to converge. However, if the learning rate is too high, the model never converges, but instead bounces around the weights and bias that minimize the loss. The goal is to pick a learning rate that's not too high nor too low so that the model converges quickly.**

The difference between the old model parameters and the new model parameters is proportional to the slope of the loss function. For example, if the slope is large, the model takes a large step. If small, it takes a small step. For example, if the gradient's magnitude is 2.5 and the learning rate is 0.01, then the model will change the parameter by 0.025.

The ideal learning rate helps the model to converge within a reasonable number of iterations. In Figure 21, the loss curve shows the model significantly improving during the first 20 iterations before beginning to converge:
![[Pasted image 20250608171049.png]]

In contrast, a learning rate that's too small can take too many iterations to converge. In Figure 22, the loss curve shows the model making only minor improvements after each iteration:
![[Pasted image 20250608171104.png]]

A learning rate that's too large never converges because each iteration either causes the loss to bounce around or continually increase. In Figure 23, the loss curve shows the model decreasing and then increasing loss after each iteration, and in Figure 24 the loss increases at later iterations:
![[Pasted image 20250608171135.png]]


## Batch size

[**Batch size**](https://developers.google.com/machine-learning/glossary#batch-size) is a hyperparameter that refers to the number of [**examples**](https://developers.google.com/machine-learning/glossary#example) the model processes before updating its weights and bias. You might think that the model should calculate the loss for _every_ example in the dataset before updating the weights and bias. However, when a dataset contains hundreds of thousands or even millions of examples, using the full batch isn't practical.

Two common techniques to get the right gradient on _average_ without needing to look at every example in the dataset before updating the weights and bias are [**stochastic gradient descent**](https://developers.google.com/machine-learning/glossary#SGD) and [**mini-batch stochastic gradient descent**](https://developers.google.com/machine-learning/glossary#mini-batch-stochastic-gradient-descent):

- **Stochastic gradient descent (SGD)**: Stochastic gradient descent uses only a single example (a batch size of one) per iteration. Given enough iterations, SGD works but is very noisy. "Noise" refers to variations during training that cause the loss to increase rather than decrease during an iteration. The term "stochastic" indicates that the one example comprising each batch is chosen at random.

	Notice in the following image how loss slightly fluctuates as the model updates its weights and bias using SGD, which can lead to noise in the loss graph:
	![[Pasted image 20250608171318.png]]

Note that using stochastic gradient descent can produce noise throughout the entire loss curve, not just near convergence.

**Mini-batch stochastic gradient descent (mini-batch SGD)**: Mini-batch stochastic gradient descent is a compromise between full-batch and SGD. For number of data points, the batch size can be any number greater than 1 and less than N. The model chooses the examples included in each batch at random, averages their gradients, and then updates the weights and bias once per iteration.

Determining the number of examples for each batch depends on the dataset and the available compute resources. In general, small batch sizes behaves like SGD, and larger batch sizes behaves like full-batch gradient descent.![[Pasted image 20250608171419.png]]

When training a model, you might think that noise is an undesirable characteristic that should be eliminated. However, a certain amount of noise can be a good thing. In later modules, you'll learn how noise can help a model [**generalize**](https://developers.google.com/machine-learning/glossary#generalization) better and find the optimal weights and bias in a [**neural network**](https://developers.google.com/machine-learning/glossary#neural-network).

## Epochs

During training, an [**epoch**](https://developers.google.com/machine-learning/glossary#epoch) means that the model has processed every example in the training set _once_. For example, given a training set with 1,000 examples and a mini-batch size of 100 examples, it will take the model 10 [**iterations**](https://developers.google.com/machine-learning/glossary#iteration) to complete one epoch.

Training typically requires many epochs. That is, the system needs to process every example in the training set multiple times.

The number of epochs is a hyperparameter you set before the model begins training. In many cases, you'll need to experiment with how many epochs it takes for the model to converge. In general, more epochs produces a better model, but also takes more time to train.
![[Pasted image 20250608171821.png]]

The following table describes how batch size and epochs relate to the number of times a model updates its parameters.

|Batch type|When weights and bias updates occur|
|---|---|
|Full batch|After the model looks at all the examples in the dataset. For instance, if a dataset contains 1,000 examples and the model trains for 20 epochs, the model updates the weights and bias 20 times, once per epoch.|
|Stochastic gradient descent|After the model looks at a single example from the dataset. For instance, if a dataset contains 1,000 examples and trains for 20 epochs, the model updates the weights and bias 20,000 times.|
|Mini-batch stochastic gradient descent|After the model looks at the examples in each batch. For instance, if a dataset contains 1,000 examples, and the batch size is 100, and the model trains for 20 epochs, the model updates the weights and bias 200 times.|
