[Linear regression: Gradient descent  |  Machine Learning  |  Google for Developers](https://developers.google.com/machine-learning/crash-course/linear-regression/gradient-descent)
# Linear regression: Gradient descent
[**Gradient descent**](https://developers.google.com/machine-learning/glossary#gradient-descent) is a mathematical technique that iteratively finds the weights and bias that produce the model with the lowest loss. Gradient descent finds the best weight and bias by repeating the following process for a number of user-defined iterations.

The model begins training with randomized weights and biases near zero, and then repeats the following steps:

1. Calculate the loss with the current weight and bias.
    
2. Determine the direction to move the weights and bias that reduce loss.
    
3. Move the weight and bias values a small amount in the direction that reduces loss.
    
4. Return to step one and repeat the process until the model can't reduce the loss any further.


The diagram below outlines the iterative steps gradient descent performs to find the weights and bias that produce the model with the lowest loss.
![[Pasted image 20250608125352.png]]
## Model convergence and loss curves

When training a model, you'll often look at a [**loss curve**](https://developers.google.com/machine-learning/glossary#loss-curve) to determine if the model has [**converged**](https://developers.google.com/machine-learning/glossary#convergence). The loss curve shows how the loss changes as the model trains. The following is what a typical loss curve looks like. Loss is on the y-axis and iterations are on the x-axis:
![[Pasted image 20250608130038.png]]
You can see that loss dramatically decreases during the first few iterations, then gradually decreases before flattening out around the 1,000th-iteration mark. After 1,000 iterations, we can be mostly certain that the model has converged.

In the following figures, we draw the model at three points during the training process: the beginning, the middle, and the end. Visualizing the model's state at snapshots during the training process solidifies the link between updating the weights and bias, reducing loss, and model convergence.

In the figures, we use the derived weights and bias at a particular iteration to represent the model. In the graph with the data points and the model snapshot, blue loss lines from the model to the data points show the amount of loss. The longer the lines, the more loss there is.

In the following figure, we can see that around the second iteration the model would not be good at making predictions because of the high amount of loss.
![[Pasted image 20250608130117.png]]

At around the 400th-iteration, we can see that gradient descent has found the weight and bias that produce a better model.
![[Pasted image 20250608130144.png]]

And at around the 1,000th-iteration, we can see that the model has converged, producing a model with the lowest possible loss.
![[Pasted image 20250608130156.png]]

### Convergence and convex functions

The loss functions for linear models always produce a [**convex**](https://developers.google.com/machine-learning/glossary#convex-function) surface. As a result of this property, when a linear regression model converges, we know the model has found the weights and bias that produce the lowest loss.

If we graph the loss surface for a model with one feature, we can see its convex shape. The following is the loss surface of the miles per gallon dataset used in the previous examples. Weight is on the x-axis, bias is on the y-axis, and loss is on the z-axis:
![[Pasted image 20250608131029.png]]

In this example, a weight of -5.44 and bias of 35.94 produce the lowest loss at 5.54:
![[Pasted image 20250608131046.png]]

A linear model converges when it's found the minimum loss. Therefore, additional iterations only cause gradient descent to move the weight and bias values in very small amounts around the minimum. If we graphed the weights and bias points during gradient descent, the points would look like a ball rolling down a hill, finally stopping at the point where there's no more downward slope.

![[Pasted image 20250608131123.png]]

Notice that the black loss points create the exact shape of the loss curve: a steep decline before gradually sloping down until they've reached the lowest point on the loss surface.

It's important to note that the model almost never finds the exact minimum for each weight and bias, but instead finds a value very close to it. It's also important to note that the minimum for the weights and bias don't correspond to zero loss, only a value that produces the lowest loss for that parameter.

Using the weight and bias values that produce the lowest loss—in this case a weight of -5.44 and a bias of 35.94—we can graph the model to see how well it fits the data:

![[Pasted image 20250608131312.png]]
