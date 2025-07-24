Here are the main steps we will walk through:
1. Look at the big picture.
2. Get the data.
3. Explore and visualize the data to gain insights.
4. Prepare the data for machine learning algorithms.
5. Select a model and train it.
6. Fine-tune your model.
7. Present your solution.
8. Launch, monitor, and maintain your system.
## Working with Real Data
When you are learning about machine learning, it is best to experiment with real-world data, not artificial datasets. Fortunately, there are thousands of open datasets to choose from, ranging across all sorts of domains. Here are a few places you can look to get data:

- Popular open data repositories:
    - [Google Datasets Search](https://datasetsearch-research-google-com.proxy.library.nyu.edu/)
    - [Hugging Face Datasets](https://huggingface.co/docs/datasets)
    - [OpenML.org](https://openml.org)
    - [Kaggle.com](https://kaggle.com/datasets)
    - [PapersWithCode.com](https://paperswithcode.com/datasets)
    - [UC Irvine Machine Learning Repository](https://archive.ics.uci.edu)
    - [Stanford Large Network Dataset Collection](https://snap-stanford-edu.proxy.library.nyu.edu/data/)
    - [Amazon’s AWS datasets](https://registry.opendata.aws)
    - [U.S. Government’s Open Data](https://data.gov/)
    - [DataPortals.org](https://dataportals.org)
    - [Wikipedia’s list of machine learning datasets](https://homl.info/9)

## Look at the Big Picture
Your model should learn from this data and be able to predict the median housing price in any district, given all the other metrics.

## Frame the Problem

The first question to ask your boss is what exactly the business objective is. Building a model is probably not the end goal. How does the company expect to use and benefit from this model? Knowing the objective is important because it will determine how you frame the problem, which algorithms you will select, which performance measure you will use to evaluate your model, and how much effort you will spend tweaking it.

Your boss answers that your model’s output (a prediction of a district’s median housing price) will be essential to determine whether or not it is worth investing in a given area. More specifically, your model’s output will be fed to another machine learning system (see [Figure 2-2](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/hands-on-machine-learning/9798341607972/ch02.html#house_pricing_pipeline_diagram)), along with some other signals.⁠[2](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/hands-on-machine-learning/9798341607972/ch02.html#id637) So it’s important to make our housing price model as accurate as we can.

The next question to ask your boss is what the current solution looks like (if any). The current situation will often give you a reference for performance, as well as insights on how to solve the problem. Your boss answers that the district housing prices are currently estimated manually by experts: a team gathers up-to-date information about a district, and when they cannot get the median housing price, they estimate it using complex rules.

This is costly and time-consuming, and their estimates are not great; in cases where they manage to find out the actual median housing price, they often realize that their estimates were off by more than 30%. This is why the company thinks that it would be useful to train a model to predict a district’s median housing price, given other data about that district. The census data looks like a great dataset to exploit for this purpose, since it includes the median housing prices of thousands of districts, as well as other data.

---
# Pipelines

A sequence of data processing components is called a data _pipeline_. Pipelines are very common in machine learning systems, since there is a lot of data to manipulate and many data transformations to apply.

Components typically run asynchronously. Each component pulls in a large amount of data, processes it, and spits out the result in another data store. Then, some time later, the next component in the pipeline pulls in this data and spits out its own output. Each component is fairly self-contained: the interface between components is simply the data store. This makes the system simple to grasp (with the help of a data flow graph), and different teams can focus on different components. Moreover, if a component breaks down, the downstream components can often continue to run normally (at least for a while) by just using the last output from the broken component. This makes the architecture quite robust.

On the other hand, a broken component can go unnoticed for some time if proper monitoring is not implemented. The data gets stale and the overall system’s performance drops.

---

With all this information, you are now ready to start designing your system. First, determine what kind of training supervision the model will need: is it a supervised, unsupervised, semi-supervised, self-supervised, or reinforcement learning task? And is it a classification task, a regression task, or something else? Should you use batch learning or online learning techniques? Before you read on, pause and try to answer these questions for yourself.


## Select a Performance Measure
Your next step is to select a performance measure. A typical performance measure for regression problems is the _root mean squared error_ (RMSE). It gives an idea of how much error the system typically makes in its predictions, with a higher weight given to large errors. [Equation 2-1](https://learning-oreilly-com.proxy.library.nyu.edu/library/view/hands-on-machine-learning/9798341607972/ch02.html#rmse_equation) shows the mathematical formula to compute the RMSE.

##### Equation 2-1. Root mean squared error (RMSE)
$$
\text{RMSE}(\mathbf{X}, \mathbf{y}, h) = \sqrt{\frac{1}{m} \sum_{i=1}^{m} \left( h(\mathbf{x}^{(i)}) - y^{(i)} \right)^2}
$$
