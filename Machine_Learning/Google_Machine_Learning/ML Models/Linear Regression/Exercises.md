# Part 1 - Setup Exercise
## Load required modules
This exercise depends on several Python libraries to help with data manipulation, machine learning tasks, and data visualization.

**Instructions**

1. Run the **Install required libraries** code cell (below).
2. Run the **Load dependencies** code cell (below).

## Load the dataset

The following code cell loads the dataset and creates a pandas DataFrame.

You can think of a DataFrame like a spreadsheet with rows and columns. The rows represent individual data examples, and the columns represent the attributes associated with each example.

## Update the dataframe

The following code cell updates the DataFrame to use only specific columns from the dataset.

Notice that that output shows just a sample of the dataset, but there should be enough information for you to identify the features associated with the dataset, and have a look at the actual data for a few examples.

# Part 2 - Dataset Exploration
## View dataset statistics

A large part of most machine learning projects is getting to know your data. In this step, you will use the `DataFrame.describe` method to view descriptive statistics about the dataset and answer some important questions about the data.

**Instructions**

1. Run the **View dataset statistics** code cell.
2. Inspect the output and answer these questions:
    - What is the maximum fare?
    - What is the mean distance across all trips?
    - How many cab companies are in the dataset?
    - What is the most frequent payment type?
    - Are any features missing data?
3. Run the code **View answers to dataset statistics** code cell to check your answers.

You might be wondering why there are groups of `NaN` (not a number) values listed in the output. When working with data in Python, you may see this value if the result of a calculation can not be computed or if there is missing information. For example, in the taxi dataset `PAYMENT_TYPE` and `COMPANY` are non-numeric, categorical features; numeric information such as mean and max do not make sense for categorical features so the output displays `NaN`.

### Code - View dataset statistics
```bash
#@title Code - View dataset statistics
print('Total number of rows: {0}\n\n'.format(len(training_df.index)))

training_df.describe(include='all')
```

### Double-click or run to view answers about dataset statistics
```bash
#@title Double-click or run to view answers about dataset statistics

  

answer = '''

What is the maximum fare? Answer: $159.25

What is the mean distance across all trips? Answer: 8.2895 miles

How many cab companies are in the dataset? Answer: 31

What is the most frequent payment type? Answer: Credit Card

Are any features missing data? Answer: No

'''
# What is the maximum fare?
max_fare = training_df['FARE'].max()
print("What is the maximum fare? \t\t\t\tAnswer: ${fare:.2f}".format(fare = max_fare))

# What is the mean distance across all trips?
mean_distance = training_df['TRIP_MILES'].mean()
print("What is the mean distance across all trips? \t\tAnswer: {mean:.4f} miles".format(mean = mean_distance))

# How many cab companies are in the dataset?
num_unique_companies = training_df['COMPANY'].nunique()
print("How many cab companies are in the dataset? \t\tAnswer: {number}".format(number = num_unique_companies))

# What is the most frequent payment type?
most_freq_payment_type = training_df['PAYMENT_TYPE'].value_counts().idxmax()
print("What is the most frequent payment type? \t\tAnswer: {type}".format(type = most_freq_payment_type))

# Are any features missing data?
missing_values = training_df.isnull().sum().sum()
print("Are any features missing data? \t\t\t\tAnswer:", "No" if missing_values == 0 else "Yes")
```

## Generate a correlation matrix

An important part of machine learning is determining which [features](https://developers.google.com/machine-learning/glossary/#feature) correlate with the [label](https://developers.google.com/machine-learning/glossary/#label). If you have ever taken a taxi ride before, your experience is probably telling you that the fare is typically associated with the distance traveled and the duration of the trip. But, is there a way for you to learn more about how well these features correlate to the fare (label)?

In this step, you will use a **correlation matrix** to identify features whose values correlate well with the label. Correlation values have the following meanings:

- **`1.0`**: perfect positive correlation; that is, when one attribute rises, the other attribute rises.
- **`-1.0`**: perfect negative correlation; that is, when one attribute rises, the other attribute falls.
- **`0.0`**: no correlation; the two columns [are not linearly related](https://www.google.com/url?q=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FCorrelation_and_dependence%23%2Fmedia%2FFile%3ACorrelation_examples2.svg).

In general, the higher the absolute value of a correlation value, the greater its predictive power.

**Instructions**

1. Inspect the code in the **View correlation matrix** code cell.
2. Run the **View correlation matrix** code cell and inspect the output.
3. **Check your understanding** by answering these questions:
    - Which feature correlates most strongly to the label FARE?
    - Which feature correlates least strongly to the label FARE?
### Double-click to view answers about the correlation matrix

```bash
#@title Double-click to view answers about the correlation matrix
# Which feature correlates most strongly to the label FARE?
# ---------------------------------------------------------
answer = '''
The feature with the strongest correlation to the FARE is TRIP_MILES.
As you might expect, TRIP_MILES looks like a good feature to start with to train
the model. Also, notice that the feature TRIP_SECONDS has a strong correlation
with fare too.

'''
print(answer)
# Which feature correlates least strongly to the label FARE?
# -----------------------------------------------------------
answer = '''The feature with the weakest correlation to the FARE is TIP_RATE.'''
print(answer)
```
![[Pasted image 20250608174449.png]]

The feature with the strongest correlation to the FARE is TRIP_MILES.
As you might expect, TRIP_MILES looks like a good feature to start with to train
the model. Also, notice that the feature TRIP_SECONDS has a strong correlation
with fare too.

The feature with the weakest correlation to the FARE is TIP_RATE.

## Visualize relationships in dataset

Sometimes it is helpful to visualize relationships between features in a dataset; one way to do this is with a pair plot. A **pair plot** generates a grid of pairwise plots to visualize the relationship of each feature with all other features all in one place.

### Code - View pairplot
```bash
#@title Code - View pairplot
sns.pairplot(training_df,
x_vars=["FARE", "TRIP_MILES", "TRIP_SECONDS"],
y_vars=["FARE", "TRIP_MILES", "TRIP_SECONDS"]
)
```

# Part 3 - Train Model
## Define functions to view model information
To help visualize the results of each training run you will generate two plots at the end of each experiment:

- a scatter plot of the features vs. the label with a line showing the output of the trained model
- a loss curve

## Define functions to build and train a model

The code you need to build and train your model is in the **Define ML functions** code cell. If you would like to explore this code, expand the code cell and take a look.
## Train a model with one feature

In this step you will train a model to predict the cost of the fare using a **single feature**. Earlier, you saw that `TRIP_MILES` (distance) correlates most strongly with the `FARE`, so let's start with `TRIP_MILES` as the feature for your first training run.

**Instructions**

1. Run the **Experiment 1** code cell to build your model with one feature.
2. Review the output from the training run
3. **Check your understanding** by answering these questions:
    - How many epochs did it take to converge on the final model?
    - How well does the model fit the sample data?

During training, you should see the root mean square error (RMSE) in the output. The units for RMSE are the same as the units for the label (dollars). In other words, you can use the RMSE to determine how far off, on average, the predicted fares are in dollars from the observed values.