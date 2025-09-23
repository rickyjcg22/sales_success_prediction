# ARCH CASE STUDY

Proponent: Ricky Jay Gomez

## Executive Summary

## Overview

Our client is an insurance company with large global operations offering insurance, reinsurance, and mortgage insurance. The company has reached out to ask for our expertise in developing a predictive model to optimize sales efforts and maximize company revenue. Initially, they conducted a review on their sales data using their dataset against their in-house developed model and predictive modeling approaches (methodology). As a predictive service provider, they tasked our team to build a predictive model using the same dataset that they used in their review process to verify if their in-house developed model provides sound business insights. There are no specific criteria about the modeling approach that we will use, but they provided a list of some important things to consider as we build our model— mostly covers topics related to data pre-processing, model training & validation, interpretability & generalization, and optimization & evaluation.

## Problem Statement

Our team achieves its success once these important business questions from our client are answered:

- How can we identify which customers are most likely to accept the sales offer when the contact center calls them, so we optimize the sales team effort and reduce wasted call?
- How can we prioritize leads so that we maximize conversion rates and sales performance?
- How will using this developed model affect overall ROI— i.e., the balance of campaign costs vs. sales revenue?

## Objectives

In this project, our team aims:

- To discriminate customers between who will or will not likely to take the sales offer once they are cold called by building a predictive model capable of classifying leads.
- To assess the financial impact of the developed model based on its classification outcomes by quantifying revenue gains, cost savings, lost opportunites, and the cost of incorrect predictions.

## Methodology and Modeling Framework

### Framework

This work follows the modeling framework in the figure below.

!["Modeling Framework"](assets/arch_framework.jpg "Figure 1: Modeling Framework")

Figure 1: Modeling Framework

Here is the step-by-step procedure of the modeling process:

- It started with the data acquisition where the dataset was requested from the client as .csv file.
- The dataset was analyzed through conducting a full-blown Exploratory Data Analysis wherein data were loaded, datatypes were inspected, and column names were fixed for better readability and processing during the EDA. Few checks were done including the Target Variable Analysis, Missing Values and Data Types, Numerical Feature Analysis, Categorical Feature Analysis, and Target-wise Analysis.
- Based on the EDA outcomes, data was pre-processed by feature transformation, scaling, and normalization. These were done to make sure that categorical features are encoded with numerical representations, and numerical features were scaled and normalized to avoid discriminating power in some features.
- The iterative process of training, evaluating, and optimizing the models was applied to make sure that we get the model with best possible outcomes, especially when profitability is considered.
- Lastly, final business insights were summarized to help our client make informed decisions about their company's sales effort and profit maximization.

### Dataset

Our client provided a sales dataset consisting of 35,000 total number of observations with 10 numerical features, 10 categorical features, and 1 binary response variable (0s for No-Buys and 1s for Buys). Based on the initial view of the response variable, there is a severe class imbalance observed wherein 88.77% belong to No-buys and only 11.23% belong to Buys. There is no information about the unique identification for each record that would help us infer about the granularity of the dataset, and temporal data for time effects. Some numerical features were scaled and normalized to make sure that any discriminating power based on the differences in their scales does not affect the model performance. Categorical features were encoded numerically, but the choice of which method should be used depends on the type of the data distribution, business context, and other factors. EDA results supplement the data pre-processing to make sure that model performance is optimized from the dataset level.

### Exploratory Data Analysis

#### Target Variable Analysis

The response rate was observed to have a severe class imbalance where No-Buys (88.77%) class has dominated Buys (11.23%) class.

#### Missing Values

There are no missing values detected across all features.

#### Numerical Feature Analysis

Table 1 shows the summary of the desciptive statistics of all the numerical features and the observations drawn from them. Whereas, Figure 2 support the summary statistics with visualized data distributions for each feature through their histograms.

Table 1: Numerical Feature Summary Statistics
| Feature | Statistics | Observations | Action Items |
| :-----: | :--------: | :----------: | :----------: |
| age | Min/Max: 17 to 98<br>Mean/Median: 40.03 vs. 38.00 | - Reasonable data range<br>- Slightly right skewed | |
| duration_latest | Mean: 257.8<br>Max: 4918<br>Std. Dev.: 258.6 | <br>- Strongly right skewed with a very long tail<br>- High variance | |
| count_call_current | Mean: 2.56<br>Max: 56<br>75%: 3 | - Most values are low with few extreme outliers | |
| days_last_campaign | Mean: 962<br>25-75%: 999 | - 999 is used to encode "no previous campaign" | |
| count_call_previous | Mean: 0.17<br>75%: 0<br>Max: 7 | - Most values are zeros<br>- Maximum value are found to be rare cases with high contact | |
| evr_quarterly | Min: -3.4<br>Max: 1.4 | - Slightly left skewed<br>- With high variance relative to mean | |
| cpi_monthly | Min/Max: 92.2 to 94.8<br>Std. Dev.: 0.58 | - Tigh range and variability | |
| cci_monthly | Min/Max: -50.8 to -26.9 | - Negative values observed (normally seen for CCIs) | |
| ibr_employee_quarterly | Min/Max: 0.63 to 5.04<br>Median: 4.86 | - Likely bimodal distribution | |
| count_employee_quarterly | Min/Max: 4963.6 to 5228.1<br>Mean: 5167.04<br>Std. Dev.: 72.17 | - Relatively low standard deviation compared to mean value cautions static, low variance feature | |

!["Histogram of Numerical Features"](assets/num_hist.png "Figure 2: Histogram of Numerical Features")

Figure 2: Histogram of Numerical Features

Based on the boxplots, numerical features such as age, duration_latest, count_call_current, days_last_campaign, and count_call_previous were found to have a lot of outliers that go beyond the extreme values. Others such as evr_quarterly, cpi_monthly, cci_monthly, ibr_employee_quarterly, and count_employee_quarterly were found to have little to no outlier in their data.

Correlation heatmaps were also analyzed to check the correlation of each feature with the target as summarized in Table 2:

- duration_latest was found to exhibit highest postive correlation with the target. It implies that customers who stay longer on the call are more likely to buy than those who do not.
- More calls in previous campaign is observed to increase the probability of a customer taking the sales offer based on the moderate positive relationship between count_call_previous and target.
- The feature cci_monthly has a weak positive correlation with the target and might not be a strong predictor of the sales outcome on its own.
- Similarly, age has been perceived to have negligible effect on the customer's buying decision as well, but indicates that older people are more likely to buy based on its positive correlation with the target.
- In contrary to the one observed from the count_call_previous, count_call_current has showed a slightly inverse effect on the customer's buying decions; meaning, frequent calling might annoy customers or reflect unproductive efforts. However, the magnitude of their relationship with the target is higher for count_call_previous which might indicate that more wins could predominate based on the previous call frequencies.
- A moderately negative effect of evr_quarterly on the target could reflect economic slowdown dampering buying behavior.
- Similar observation was seen with ibr_quarterly as with the evr_quarterly which might be possibly linked to economoic conditions.
- Moderately negative effect of the number of employees was seen which may indicate structural economy-wide trends.
- A weak negative effect of cpi_monthly on the buying decision could imply that customers might be sensitive to price changes and inflation, but other factors might be influential to their final buying behavior.

Table 2: Numerical Feature to Target Correlation
| Feature | Correlation Coefficient | Observations |
| :-----: | :--------: | :----------: |
| age | +0.03 | Weak positive|
| duration_latest | +0.41 | Strong positive |
| count_call_current | –0.07 | Weak negative |
| days_last_campaign | –0.32 | Moderate negative |
| count_call_previous | +0.23 | Moderate positive |
| evr_quarterly | –0.30 | Moderate negative |
| cpi_monthly | –0.14 | Weak negative |
| cci_monthly | +0.05 | Weak positive |
| ibr_employee_quarterly | –0.31 | Moderate negative |
| count_employee_quarterly | –0.35 | Moderate negative |

Correlation among the features were also investigated to verify if multi-collinearity exist which could affect the model performance. The highest correlation among numerical features was seen between evr_quarterly and ibr_employee_quarterly with correlation coefficient of +0.97, indicating a strong positive relationship between them. On the other hand, ibr_employee_quarterly and count_employee_quarterly were seen to have a strong positive correlation of +0.95. Whereas, evr_quarterly and count_employee_quarterly with another strong positive correlation of +0.91. Although not as strong as the other three relationships, a significant correlation between the days_last_campaign and the count_call_previous was also seen with correlation coefficient value of +0.59. To confirm the reliability of these observations, Variance Inflation Factor (VIF) analysis was also conducted. The result showed that ibr_employee_quarterly, evr_quarterly, and count_employee_quarterly are the numerical features with the highest VIF values (greater than 10), which is aligned to the initial observation of multi-collinearity via correlation analysis.

#### Categorical Feature Analysis

### Numerical Feature Transformation

### Categorical Feature Transformation

### Train-Test Split

### Training Algorithms

## Model Development

## Model Evaluation

## Model Optimization: Hyperparameter Tuning and Cross Validation

## Results and Discussions

## Conclusions

## Recommendation
