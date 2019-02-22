---
layout: post
category: post
title: Machine Learning - Part I - Explorative Data Analysis (EDA)
date: 2018-10-22
excerpt: ""
tags: []
comments: true
---

[DRAFT!!]
# Agenda

- Introduction
- Data Types
- Descriptive Statistics
- Visual Analytics
- Data Preparation

Objective of this post is understanding:
- The methods of EDA
- The difference between EDA and machine learning

# tl;dr

## What is EDA?
[In statistics, exploratory data analysis (EDA) is an approach to analyzing data sets to summarize their main characteristics, often with visual methods](https://en.wikipedia.org/wiki/Exploratory_data_analysis). In explorative data analysis data are described using statistical models and visual methods.

Synonyms:
- [data wrangling/data munging](https://en.wikipedia.org/wiki/Data_wrangling): process of transforming and mapping raw data into a more appropriate and valuable format.
- [data profiling](https://en.wikipedia.org/wiki/Data_profiling): process of examining data and collecting statistics or informative summaries.

## Data Types

### Example: data about customer creditworthiness
<img src="{{site.baseurl}}/assets/img/2018-10-22-machine-learning-1-1-example.png">
Attributes are also called characteristics, features, properties. A data set is called an instance of a data set.

### Data can be categorized into these types:
<img src="{{site.baseurl}}/assets/img/2018-10-22-machine-learning-1-2-data-types.png">
Source: [Stevens, S. S. Stevens, Science, New Series, Vol. 103, No. 2684. (Jun. 7, 1946), pp. 677-680.](https://marces.org/EDMS623/Stevens%20SS%20(1946)%20On%20the%20Theory%20of%20Scales%20of%20Measurement.pdf)

### Depending on the types these operations are possible:
<img src="{{site.baseurl}}/assets/img/2018-10-22-machine-learning-1-3-data-operations.png">

There are several possibilities of representing data such as [one-hot encoding](https://machinelearningmastery.com/why-one-hot-encode-data-in-machine-learning/):
<img src="{{site.baseurl}}/assets/img/2018-10-22-machine-learning-1-4-representations.png">

## Descriptive Statistics

For describing numeric data there are location and scale parameters/properties.

Example data visualized in a [histogram](https://en.wikipedia.org/wiki/Histogram):
<img src="{{site.baseurl}}/assets/img/2018-10-22-machine-learning-2-1-histogram-example.png">

### Location parameters: [mean (average)](https://en.wikipedia.org/wiki/Mean) and [median](https://en.wikipedia.org/wiki/Median) value.
<img src="{{site.baseurl}}/assets/img/2018-10-22-machine-learning-2-2-mean-median.png">

Example of global wealth: mean values are sensitive whereas median values are robust against outliers
<img src="{{site.baseurl}}/assets/img/2018-10-22-machine-learning-2-3-global-wealth-databook.png">
Source: [Credit Suisse Global Wealth Databook 2017](http://rogerannis.com/wp-content/uploads/2017/11/Credit-Suisse-Global-Wealth-Databook-2017.pdf)

### Scale parameters: [Variance](https://en.wikipedia.org/wiki/Variance#Sample_variance) and [standard deviation](https://en.wikipedia.org/wiki/Standard_deviation)

Example: [scatter plot](https://en.wikipedia.org/wiki/Scatter_plot)
<img src="{{site.baseurl}}/assets/img/2018-10-22-machine-learning-2-4-variance-standard-deviation.png">
- variance (special case of covariance) is the squared deviation of a random variable from its mean, it measures how far a set of (random) numbers are spread out from their average value. 
- standard deviation is the square root of its variance, low standard deviation = close to the mean, high standard deviation = spread out over a wider range of values.

### [Covariance](https://en.wikipedia.org/wiki/Covariance#Calculating_the_sample_covariance) and [Correlation](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient)
<img class="fit image" src="{{site.baseurl}}/assets/img/2018-10-22-machine-learning-2-5-covariance-correlation.png">
Two variables/attributes and their linear relationship can be measured using:
- covariance as a measure of their joint variability
- correlation is the normalized covariance with a value between -1 and 1, where 1 is a very strong, -1 a very weak, and 0 no relation between x and y.

[Scatterplot example 1 of correlation](https://upload.wikimedia.org/wikipedia/commons/3/34/Correlation_coefficient.png)
<img class="fit image" src="{{site.baseurl}}/assets/img/2018-10-22-machine-learning-2-5-wikipedia-covariance.png">
[Scatterplot example 2 of correlation](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient#/media/File:Correlation_examples2.svg)
<img class="fit image" src="{{site.baseurl}}/assets/img/2018-10-22-machine-learning-2-5-wikipedia-Correlation_examples2.svg">





# Introduction

https://www.youtube.com/watch?v=pkGET-OIVg8&feature=youtu.be

> In statistics, exploratory data analysis (EDA) is an approach to analyzing data sets to summarize their main characteristics, often with visual methods. A statistical model can be used or not, but primarily EDA is for seeing what the data can tell us beyond the formal modeling or hypothesis testing task. Exploratory data analysis was promoted by John Tukey to encourage statisticians to explore the data, and possibly formulate hypotheses that could lead to new data collection and experiments.
Source: [Explorative Data Analysis, Wikipedia](https://en.wikipedia.org/wiki/Exploratory_data_analysis)

... tries finding answers about:
- what data types?
- what are there distribution?
- are there runaways and gaps in the data?
- are there correlations between each characteristics?

In explorative data analysis data are described using statistical models and visual methods.

## CRISP model - Cross-Industry Standard Process for Data Mining

> CRISP-DM breaks the process of data mining into six major phases.<br>
> The sequence of the phases is not strict and moving back and forth between different phases as it is always required. The arrows in the process diagram indicate the most important and frequent dependencies between phases. The outer circle in the diagram symbolizes the cyclic nature of data mining itself. A data mining process continues after a solution has been deployed. The lessons learned during the process can trigger new, often more focused business questions, and subsequent data mining processes will benefit from the experiences of previous ones.
Souce: [Wikipedia](https://en.wikipedia.org/wiki/Cross-industry_standard_process_for_data_mining#Major_phases)
<img src="{{site.baseurl}}/assets/img/2018-10-22-machine-learning-1-CRISP-DM_Process_Diagram.png">

I found this nice [blog post](https://www.virtulytix.com/intel/2018/3/15/crisp-dm-the-scrum-agile-way-why-not) about doing CRISP-DM the Scrum Agile way.

Explorative data anlysis in the CRISP-DM model lies between the phase data understanding and data preparation.

## Synonyms
- [Data wrangling](https://en.wikipedia.org/wiki/Data_wrangling), sometimes referred to as data munging, is the process of transforming and mapping data from one "raw" data form into another format with the intent of making it more appropriate and valuable for a variety of downstream purposes such as analytics.
- [Data profiling](https://en.wikipedia.org/wiki/Data_profiling) is the process of examining the data available from an existing information source (e.g. a database or a file) and collecting statistics or informative summaries about that data. The purpose of these statistics may be to:
  - Find out whether existing data can be easily used for other purposes
  - Improve the ability to search data by tagging it with keywords, descriptions, or assigning it to a category
  - Assess data quality, including whether the data conforms to particular standards or patterns[2]
  - Assess the risk involved in integrating data in new applications, including the challenges of joins
  - Discover metadata of the source database, including value patterns and distributions, key candidates, foreign-key candidates, and functional dependencies
  - Assess whether known metadata accurately describes the actual values in the source database
  - Understanding data challenges early in any data intensive project, so that late project surprises are avoided. Finding data problems late in the project can lead to delays and cost overruns.
  - Have an enterprise view of all data, for uses such as master data management, where key data is needed, or data governance for improving data quality.
 
# Data Types

Example: data about customer creditworthiness

|age              |family status    |income level     |employeed since  |credit limit     |usage            |creditworthy?    |
|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|
|46               |married          |high             |2001             |20'000           |car              |yes              |
|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|
|54               |married          |medium           |2007             |5'000            |other            |yes              |
|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|
|25               |single           |low              |2012             |35'000           |car              |no               |
|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|
|35               |divorced         |medium           |2005             |4'000            |equipment        |yes              |
|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|
|...              |                 |                 |                 |                 |                 |                 |
|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|


These different data are called: characteristics, attributes, features
A Data set is called an instance of a data set.

Where the attributes are:
- numeric: age, employeed since, credit limit
- nominal (text, no order): family status, use for
- ordinal (ordered like low, medium, high): income level

Data types
- numeric data
 - continuous data (temperature, credit limit)
 - discrete data (age in years)
- categorial data
 - nominal (name, use for)
 - ordinal (income level)

[Stevens, S. S. Stevens, Science, New Series, Vol. 103, No. 2684. (Jun. 7, 1946), pp. 677-680.](https://marces.org/EDMS623/Stevens%20SS%20(1946)%20On%20the%20Theory%20of%20Scales%20of%20Measurement.pdf)

Another good post explaining nominal, ordinal, interval, ration, continuous, and discrete can be found [here](https://communitymedicine4asses.com/2013/01/13/scales-of-measurement-nominal-ordinal-interval-ratio/). In short:
- Qualitative data
  - Nominal scale
    - categories are nominated names (hence “nominal”), no inherent order between categories, one cannot say that a particular category is superior/ better than another
    - example: Gender (Male/ Female), Blood Groups (A/B/O/AB), Religion (Hindu/ Muslim/ Christian/ Buddhist, etc.)
  - Ordinal scale
    - categories logically arranged in a meaningful order. However, the difference between the categories is not “meaningful”.
    - example: ranks (1st/ 2nd/ 3rd, etc.), However, the difference between ranks is not the same-the difference between the 1st rank and 2nd rank may be 20 units, but that between the 2nd and 3rd ranks may be 3 units.
- Quantitative
  - Interval scale
    - values (not categories) can be ordered and have a meaningful difference, but doubling is not meaningful. This is because of the absence of an “absolute zero”.
    - Example: The Celsius scale: The difference between 40 C and 50 C is the same as that between 20 C and 30 C (meaningful difference = equidistant). Besides, 50 C is hotter than 40 C (order). However, 20 C is not half as hot as 40 C and vice versa (doubling is not meaningful).
  - Ratio scale
    - The values can be ordered, have a meaningful difference, and doubling is also meaningful. There is an “absolute zero”.
    - Example: The Kelvin scale: 100 K is twice as hot as 50 K; the difference between values is meaningful and can be ordered.

In addition, quantitative data may also be classified as being either Discrete or Continuous.
- Discrete
  - values can be specific numbers only. Fractions are meaningless
  - Example: Number of children: 1, 2, 3, etc. are possible, but 1.5 children is not meaningful.
- Continuous
  - Any numerical value (including fractions) is possible and meaningful.
  - Example: Weight: 1kg, 1.0kg, 1.000 kg, 1.00001 kg are all meaningful. The level of precision depends upon the equipment used to measure weight.


Operations on data types
- nominal: equal, not equal
- ordinal: equal, not equal, greater/less than
- numeric: equal, not equal, greater/less than, +, -, ...

Data may have different representations and sometimes needs to be transformed:
- age: from year of birth to actual age
- family status: 1=single, 2=married, 3=divorced
- income level: 1=low, 2=medium, 3=high

Attention here: family status and income level have now numeric representations due to integer encoding. Since there are numerical all kinds of operation can be done by machine learning alghorythms. In case of family status it is a categorial order where integer encoding is not enough and a one-hot encoding will be used. One-hot encoding is a binary representation for possible cases. For family status there are 3 possible cases: single, married, divorced. Here 3 binary variables are need for a one-hot-encoding:

|age      |single   |married  |divorced |
|---------|---------|---------|---------|
|46       |0        |1        |0        |
|---------|---------|---------|---------|
|54       |0        |1        |0        |
|---------|---------|---------|---------|
|25       |1        |0        |0        |
|---------|---------|---------|---------|
|35       |0        |0        |1        |


More about [integer and one-hot encoding in this blog](https://machinelearningmastery.com/why-one-hot-encode-data-in-machine-learning/).


# Descriptive Statistics
Video: https://www.youtube.com/watch?v=cgLZ1mvSk5E&feature=youtu.be

How can we describe numeric data? What is there distribution, mean, median etc.? 

## Numeric Data: properties for a numeric characteristic

For describing numeric data there are location and scale parameters/properties:
- [Location Parameter](https://en.wikipedia.org/wiki/Location_parameter): In statistics, a location family is a class of probability distributions that is parametrized by a scalar- or vector-valued parameter x_{0} , which determines the "location" or shift of the distribution.
  - [Mean](https://en.wikipedia.org/wiki/Mean): For a data set, the arithmetic mean, also called the mathematical expectation or average, is the central value of a discrete set of numbers: specifically, the sum of the values divided by the number of values. 
  - [Median](https://en.wikipedia.org/wiki/Median): The median is the value separating the higher half from the lower half of a data sample. For example, in the data set {1, 3, 3, 6, 7, 8, 9}, the median is 6, the fourth largest, and also the fourth smallest, number in the sample.https://en.wikipedia.org/wiki/Histogram.
  - Generally speaking: a mean value are sensitive against outliers, where a median value is robust
  - [Mode](https://en.wikipedia.org/wiki/Mode_(statistics)): The mode of a set of data values is the value that appears most often.
- [Scale Parameter](https://en.wikipedia.org/wiki/Scale_parameter) is a special kind of numerical parameter of a parametric family of [Probability distribution](https://en.wikipedia.org/wiki/Probability_distribution)s. The larger the scale parameter, the more spread out the distribution.
  - [Variance](https://en.wikipedia.org/wiki/Variance#Sample_variance): In probability theory and statistics, variance is the expectation of the squared deviation of a random variable from its mean. Informally, it measures how far a set of (random) numbers are spread out from their average value.
  - [Standard deviation](https://en.wikipedia.org/wiki/Standard_deviation): a measure that is used to quantify the amount of variation or dispersion of a set of data values. A low standard deviation indicates that the data points tend to be close to the mean (also called the expected value) of the set, while a high standard deviation indicates that the data points are spread out over a wider range of values. The standard deviation of a random variable, statistical population, data set, or probability distribution is the square root of its variance.

- [Covariance](https://en.wikipedia.org/wiki/Covariance#Calculating_the_sample_covariance): a measure of the joint variability of two random variables. If the greater values of one variable mainly correspond with the greater values of the other variable, and the same holds for the lesser values, (i.e., the variables tend to show similar behavior), the covariance is positive. In the opposite case, when the greater values of one variable mainly correspond to the lesser values of the other, (i.e., the variables tend to show opposite behavior), the covariance is negative.
- [Correlation resp. Pearson correlation coefficient](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient): is a measure of the linear correlation between two variables X and Y. Owing to the Cauchy–Schwarz inequality it has a value between +1 and −1, where 1 is total positive linear correlation, 0 is no linear correlation, and −1 is total negative linear correlation. It is widely used in the sciences. 

Here is a great answer explaining the [difference of covariance and correlation](https://stats.stackexchange.com/a/18094) for dummies:




# Visual Analytics
https://www.youtube.com/watch?v=SiUE89QH0VY&feature=youtu.be

[Scatterplot](https://en.wikipedia.org/wiki/Scatter_plot)

[Anscombe's quartet](https://en.wikipedia.org/wiki/Anscombe%27s_quartet)
Anscombe's quartet comprises four datasets that have nearly identical simple descriptive statistics, yet appear very different when graphed.
<img src="{{site.baseurl}}/assets/img/2018-10-22-machine-learning-3-1-anscombe's-quartet-dataset.png">
All four sets are identical when examined using simple summary statistics, but vary considerably when graphed.

|Property                                               |Value  |Accuracy|
|-------------------------------------------------------|-----------------|---------------------------------------|
|Mean of x                                              |9                |exact                                  |
|-------------------------------------------------------|-----------------|---------------------------------------|
|Sample variance of x                                   |11               |exact                                  |
|-------------------------------------------------------|-----------------|---------------------------------------|
|Mean of y                                              |7.50	            |to 2 decimal places                    |
|-------------------------------------------------------|-----------------|---------------------------------------|
Sample variance of y                                    |4.125            |±0.003                                 |
|-------------------------------------------------------|-----------------|---------------------------------------|
|Correlation between x and y                            |0.816            |to 3 decimal places                    |
|-------------------------------------------------------|-----------------|---------------------------------------|
|Linear regression line                                 |y = 3.00 + 0.500x|to 2 and 3 decimal places, respectively|
|-------------------------------------------------------|-----------------|---------------------------------------|
|Coefficient of determination of the linear regression  |0.67             |to 2 decimal places                    |
|-------------------------------------------------------|-----------------|---------------------------------------|

Diagrams:
- The first scatter plot (top left) appears to be a simple linear relationship, corresponding to two variables correlated and following the assumption of normality.
- The second graph (top right) is not distributed normally; while a relationship between the two variables is obvious, it is not linear, and the Pearson correlation coefficient is not relevant. A more general regression and the corresponding coefficient of determination would be more appropriate.
- In the third graph (bottom left), the distribution is linear, but should have a different regression line (a robust regression would have been called for). The calculated regression is offset by the one outlier which exerts enough influence to lower the correlation coefficient from 1 to 0.816.
- Finally, the fourth graph (bottom right) shows an example when one outlier is enough to produce a high correlation coefficient, even though the other data points do not indicate any relationship between the variables.
<img src="{{site.baseurl}}/assets/img/2018-10-22-machine-learning-3-2-anscombe's-quartet-diagram.png">



Histogram

# Data preparation
Video: https://www.youtube.com/watch?v=NzuONRc_zgw&feature=youtu.be
