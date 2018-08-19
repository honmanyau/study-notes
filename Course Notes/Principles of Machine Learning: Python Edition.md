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

### Data Preparation

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




## Resources
