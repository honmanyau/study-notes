# Principles of Machine Learning: Python Edition

## Table of Contents

* [Principles of Machine Learning: Python Edition](#principles-of-machine-learning:-python-edition)
  * [Disclaimer](#disclaimer)
  * [Notes](#notes)
  * [Resources](#resources)

## Disclaimer

Everything in this file is derived from going through [Principles of Machine Learning: Python Edition](https://www.edx.org/course/principles-of-machine-learning-python-edition) and is my own work. It is meant for revision and reference purposes and I take no responsibilities for any damaged caused by the use of it (see [LICENSE](https://github.com/honmanyau/study-notes/blob/master/LICENSE.md)). If you are stupid enough to plagiarise my work then I hope you get caught and fail at everything else in life.

## Notes

### Module 1: Introduction to Machine Learning

#### Meet Your Instructor

Instructors:
* Cynthia Rudin
* Steve Elston

#### High Level Process

KDD, CRISP-DM, CCC Big Data Pipeline—attempts to scientifically formalise the process of knowledge discovery.

**[CCC Big Data Pipeline](https://cra.org/ccc/wp-content/uploads/sites/2/2015/05/bigdatawhitepaper.pdf)**

1. Acquisition/recording
2. Extraction/cleaning/annotation (reduce noise and remove unnecessary data)
3. Integration/aggregation/representation (setup data in one repository for data mining)
4. Analysis/modelling
5. Interpretation

**KDD (Knowledge Discovery and Databases)**
1. Selection
2. Preprocessing
3. Transformation
4. Data mining
5. Interpretation/evaluation

**CRISP-DM**

Similar to the above but includes business understanding and data understanding, and indication of iterative steps.

**Typical process (example)**

* Define what is to be accomplished
* Define constraints
* Identify risks
* Define how results will be evaluated
* Identify sources of data
* Data cleaning and transformation
* Model building (predictive), some examples:
  * Classification: yes/no
  * Regression: numerical value
  * Clustering: grouping observations
  * Recommendation
* Policy construction (prescriptive)
* Evaluation

**Four Vs of big data**
* Velocity
* Variety
* Volume
* Veracity (how trustworthy some data is)

#### Overview of Machine Learning with K-Means Classifiers

* e.g. f(x) = y, where x is case, y is the prediction
* If we know what y's are when training, those are called labels
* Supervised ML—learning using labels (correct answers)
* Unsupervised ML—correct answer not known

**K-Nearest Neighbour (KNN) Machine Learning Algorithm**

* Classification
* Does not require training
* New case based on proximity to labelled cases
  * Determine distance from new cases to existing cases
  * Euclidian distances, weighted distance
* K = number of neighbours used for the the prediction

Disadvantage:
* Suffers severely from curse of dimensionality

#### Demo of K-Means Classification

Refer to [/Module1/IntroductionToMachineLearning.ipynb](https://pomlpython-honmanyau.notebooks.azure.com/nb/notebooks/Module1/IntroductionToMachineLearning.ipynb)

### Module 2: Exploring Data

#### Data Exploration

An **iterative** process in understanding the data before performing machine learning.

* Characteristics of the data
* Summary statics of the dataset
* Errors and outliers (the latter may be interesting data)
* Distribution of features and labels
* Frequencies of categorical features
* Relationship between features and labels

#### Data Visualisation

A powerful method for data exploration.

#### Visualizing Distributions

#### Visualizing Data Relationships

Dealing with overplotting:
* Transparency
* 2D Kernel Density Estimate (contour)
* Hex Spin (~2D histogram)

#### Visualizing Categorical Relationships

* Box plot
* Violin plot (area under each set of curves is the same)

#### Using Aesthetics to Visualize High Dimensions

Another dimension can be added using (optionally a combination of):
* Shape
* Size
* Colour

#### Visualizing High Dimensional Relationships

* Pairwise Scatter Plot
* Conditioned/Faceted Plot

#### Visualizing for Classification

#### Frequency Tables

For examining distribution and frequency of data numerically, which is important for determining the significance of subsets of data. Example: finding class imbalance in classification.

### #Module 3: Cleaning and Preparing Data

#### Data Preparation

Data preparation ensures that machine learning algorithms work optimally on a given set of data and is vital to learning performance.

* Data exploration
* Remove duplicates
* Treat missing values
* Handle errors and outliers
* Scale features
* Split dataset
* Visualisation

#### Duplicates

Introduces bias.

* Identify duplicates
* Develop removal strategies

#### Missing Values

* Typically detected in data exploration

#### Errors and Outliers

Errors are, well, errors. Outliers are potentially valuable depending on what one is attempting to achieve with the data.

#### Scaling

Introduces biases that depend on absolute values.

* Z-Score scaling
  * mean = 0, S.D. approx. 1
* Min-max
  * scales values according to min-max
  * messes things up when outliers are present

#### Data Splitting

* Usually split before training so that test and evaluation sets are different
* Not splitting will lead to information leakage
* Examples:
  * Bernoulli sampling
  * Cross validation (resampling method)

#### Finding and Treating Missing Data

#### Finding and Treating Duplicates

#### Scaling Data

* Must split data first, then train scaler on training set, and apply scale to evaluation data

#### Overview of Feature Engineering

* Understand data relationships through data exploration
* Transform features
* Compute interaction terms
* Visualisation to check results
* Test with ML models
* Rinse and repeat

#### Transforming Features

Common transformations:
  * log, exp, square, sqrt, variance... etc.
  * Diff, cumulative sum... etc.
  * Nonlinearly transformed features are not colinear with pre and post transformation

#### Interaction Terms

Black magic.

In all seriously, the bottom line is that there is no recipe for this. Depends on domain knowledge, how well one knows about a problem, whether or not one is experienced and/or lucky enough to identify features... etc.

### Module 4: Getting Started with Supervised Learning

#### Introduction to Linear Regression

**Simple linear regression**

* One feature (x)
* SSE (Squared Standard Error)/MSE (Mean Square Error)
* Fitting f(x) = a + bx by choosing values for a and b such that SSE is minimised

#### Multiple Linear Regression

The same as simple linear regression, exception that f(x) how has multiple terms and coefficients.

#### Basics of Scikit Learn

Ehhhhhh!? Why not this video earlier?

#### Evaluating Regression Models

* MAE (Mean Absolute Error)
* RMSE (Root Mean Square Error)
* Ideally most errors should be closed to zero
* If an unusual pattern (for example, where not all of the residuals are normally distributed with a mean of 0 for linear models) exists in a residual count vs. residual plot, the model is likely imperfect
* Plotting residual vs. feature (perhaps even higher-dimension plots) may be useful in identifying how a model can be improved

#### Demo of Evaluating Regression

* Looking at regression metrics (RMS, RMSE, MAE, SSE, R^2, Adjusted R^2... etc.)
* Residual Histogram
* Quantile-quantile Plot (residual vs. predicted value)
* Residual Plot

#### Models with Categorical Variables

#### Introduction to Classification

#### Loss Function for Classification

A function that describes how "correct" a classification is.

#### Statistical Learning Theory for Supervised Learning

* Ockham's Razor: "the best models are simple models that fit the data well"
* Complex models tend to over-fit: training errors can be easily minimised but test error increases
* Simpler models tend to under-fit: does badly for both training and test errors
* Optimal model therefore neither overfit or under-fit
* Choose function *f* that minimise training error **and** complexity

#### Logistic Regression

* Minimises the loss function `log(1 + e^(-yf(x)))`

#### Maximum Likelihood Perspective

#### Evaluating Classifiers

* True negative: -1 && -1
* True positive: 1 && 1
* False Positive ("Type I"): -1 1
* False Negative ("Type II"): 1 -1
* Confusion matrix summarises the above and gives an indication of the quality of a classifier
* Classification error/misclassification rate: `(FP + FN) / n`
* True Positive Rate (TPR)/Sensitive/Recall: `TP / (TP + FN)`
* True Negative Rate (TNR)/Specificity: `TP / (TN + FP)`
* False Positive Rate (FPR): `FP / (FP + TN)`
* Precision: `TP / (TP + FP)``
* F1-score: `2 * Precision * Recall / (Precision + Recall)`

#### ROC Curves

For a particular FPR, what is the TPR?

* Area under ROC (AUROC)
* 45 degree line is random guessing
* A good model has a TP curve that increases rapidly and then plateaus at 1

#### ROC Curve Algorithms

* For a single classification model, an ROC is generated by changing `x` in `f(x)`, then recording and plotting TPR and FPR
* To evaluate an algorithm, adjust the `c` value instead

#### Classification Models

#### Demo of Classifier Evaluation

#### Imbalanced Data

* Don't just report accuracy, report TPR and FPR instead
* Splitting loss function and applying a factor to favour a particular classification/threshold

#### Approaches for Addressing Imbalanced Data

* Under sample the majority cases
  * Bernoulli sample to limit bias
  * Set probability threshold to achieve under-sampling as needed
* Over sample minority cases
  * Similar to under sampling
  * Use loop until the number of cases are obtained (???)
* Use weights on the algorithm

### Module 5: Improving Model Performance

#### Improving Models

**Greedy Backward Selection**

* Start with all features
* Remove the feature that reduces the predictive power the least
* Rinse and repeat until some arbitrary criteria are met

**Greedy Forward Selection**

* Start with no features
* Find the feature that is the best as a model by itself
* Add another one where the combined outcome is the best
* Rinse and repeat until some arbitrary criteria are met

Use adjusted R^2 to compare the different models above; R^2 is agnostic to the number of terms involved.

#### Regularization

* Regularisation limits the complexity of a model
* Regularisation term measures the simplicity of a model
* `C` determines the relative importance of accuracy and simplicity
* Linear example:
  * Sum of coefficients should be small
  * One option is to calculate the sum of the squares of the coefficients (l2 norm/l2 regularisation)
  * Another is to sum the absolute values of the coefficients (l1 norm/l1 regularisation)
* l2 regularisation tends to make all coefficients smaller
* l1 useful for making sparse solutions
* Regularisation is also known as shrinkage
* l2 term is also called Ridge regression
* l1 term is also called Lasso penalty

#### Interpreting Features

Interpreting coefficients of ML model is a bad idea:
* Collinear are highly correlated features
* Highly correlated coefficients that are large but cancel out each other could lead to incorrect interpretation
* Highly correlated features that are related to each other could be combined and still have equal predictive performance, in this case the importance of feature could be misleading
* Bad scaling means different coefficients that have the same value could have vastly different weighting
  * Bad scaling actually also messes up regularisation since different weightings are given to coefficients, and reduction of larger coefficients will be preferred

#### Features Selection

Feature selection is used for dealing with overfitted/overparameterised ML models.

#### Sweeping Parameters

#### Cross Validation

* For evaluating the performance of a ML algorithm on a dataset
* Divide data into k folds
* Train data on k - 1 folds and evaluate on the test fold
* Repeat k times using each fold as a test fold once
* Report mean and SD of all test folds

#### Nested Cross Validation

* For tuning parameters of a algorithm
* Need a dataset, algorithm, an evaluation measure and a parameter K that needs tuning
* Use one fold as test fold
* Within training set, divided into nested training set and one fold as validation set
* Rotate validation set amongst nest training set and for each value of K, train on the nested training set and evaluate on the validation fold
* Repeat k - 1 times by rotating the validation fold and the nested training set using different fold as validation fold
* Choose K that minimises the average training error over the k - 1 folds, evaluate the test set with this K
* Rinse and repeat
* Report mean and SD

#### Model Selection and Cross Validation

#### Overview of Dimensionality Reduction

* To find a small number of transformed features that contains most useful information
* PCA—find the fewest components that explain the variance of the data
* Pitfalls of PCA
  * PCA is a linear model
  * Non-linear data, for example with many modes or separated cluster may suffer
  * Outliers may substantially skew results

#### Principal Component Analysis Demo














## Resources
