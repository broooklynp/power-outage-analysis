# Analysis of Major Power Outages in the U.S.

by Brooklyn Pagaza

---

## Introduction

The dataset used for this analysis contains data about major power outages that occurred in the continental U.S. from January 2000 to July 2016. It includes one row for every observed major power outage, and contains information for each outage including the location, cause, date, and duration of the outage. In this analysis, I will be exploring the question: Which characteristics are associated with power outages in different regions of the U.S.?

Knowing about the causes of power outages across the United States can help citizens and the government decrease the likelihood of power outages occurring in the future. If there are observable patterns that point to a power outage occurring, action can be taken ahead of time to prevent a major power outage. Importantly, the characteristics that correspond with a major power outage might be different in different regions of the U.S. due to factors like weather, geography, or population, so it is important to understand those trends in order to be prepared. Since power is so essential to our lives today, it makes sense that we should use the information available to try to prevent major power outages.

The raw dataset used for this analysis contains 1534 rows. I will be using the following columns in my analysis:

| Column                         | Description                          |
|:-------------------------------|-------------------------------------:|
| `YEAR`                         | Year when the outage occurred        |
| `MONTH`                        | Month when the outage occurred       |
| `U.S._STATE`                   | State where the outage occurred      |
| `NERC.REGION`                  |North American Electric Reliability Corporation (NERC) region where the outage occurred |
| `CLIMATE.REGION`               |National Centers for Environmental Information Climate Region where the outage occurred |
| `ANOMALY.LEVEL`                | Oceanic El Niño/La Niña (ONI) index at the time of the outage|
| `CLIMATE.CATEGORY`             | State of the climate at the time of the outage |
| `OUTAGE.START.DATE`            | Day of the year when the outage started |
| `OUTAGE.START.TIME`            | Time of the day when the outage started |
| `OUTAGE.RESTORATION.DATE`      | Day of the year when power was restored |
| `OUTAGE.RESTORATION.TIME`      | Time of the day when power was restored |
| `CAUSE.CATEGORY`               | Category of the event that caused the outage |
| `OUTAGE.DURATION`              | Duration of outage in minutes |
| `DEMAND.LOSS.MW`               | Peak demand lost during an outage in megawatts |
| `CUSTOMERS.AFFECTED`           | Number of customers affected by the outage |
| `RES.SALES`                    | Residential electricity consumption |
| `COM.SALES`                    | Commercial electricity consumption |
| `IND.SALES`                    | Industrial electricity consumption |
| `TOTAL.SALES`                  | Total electricity consumption in state where outage occurred|
| `RES.CUSTOMERS`                | Number of customers served in the residential electricity sector |
| `COM.CUSTOMERS`                | Number of customers served in the commercial electricity sector |
| `IND.CUSTOMERS`                | Number of customers served in the industrial electricity sector |
| `TOTAL.CUSTOMERS`              | Total customers served in state where outage occurred |
| `PC.REALGSP.STATE`             | Per capita real gross state product |
| `TOTAL.REALGSP`                | Real GSP contributed by all industries |
| `POPULATION`                   | State population at time when outage occurred |
| `POPPCT_URBAN`                 |Percentage of the total population of the state that lives in urban communities |

---

## Data Cleaning and Exploratory Data Analysis
### Cleaning

There were multiple steps in the data cleaning process for the power outages dataset. The first step was to combine the columns `OUTAGE.START.DATE` and `OUTAGE.START.TIME` into one column called `OUTAGE.START`, which contains pd.Timestamp objects that represent the start of the outage. I also combined the `OUTAGE.RESTORATION.DATE` and `OUTAGE.RESTORATION.TIME` columns into a column called `OUTAGE.RESTORATION`, which represents the end of the outage. Then, I dropped the original date and time columns because the information was now stored in the new columns that I created.

The next step in the data cleaning process was to transform all missing data to np.NaN. I look for entries of 0 in the columns `OUTAGE.DURATION`, `DEMAND.LOSS.MW`, and `CUSTOMERS.AFFECTED` and replaced them with np.NaN, because extra 0s in the data could affect my analysis.

The first few rows of the cleaned dataframe are shown below, with a select sample of the columns:

|   YEAR |   MONTH | U.S._STATE   | NERC.REGION   | CLIMATE.REGION     |   ANOMALY.LEVEL |   OUTAGE.DURATION |   POPULATION |
|-------:|--------:|:-------------|:--------------|:-------------------|----------------:|------------------:|-------------:|
|   2011 |       7 | Minnesota    | MRO           | East North Central |            -0.3 |              3060 |      5348119 |
|   2014 |       5 | Minnesota    | MRO           | East North Central |            -0.1 |                 1 |      5457125 |
|   2010 |      10 | Minnesota    | MRO           | East North Central |            -1.5 |              3000 |      5310903 |
|   2012 |       6 | Minnesota    | MRO           | East North Central |            -0.1 |              2550 |      5380443 |
|   2015 |       7 | Minnesota    | MRO           | East North Central |             1.2 |              1740 |      5489594 |

### Univariate Analysis

I created a bar graph that shows the number of outages that occurred in each climate region.

<iframe src="assets/univariate.html" width=800 height=600 frameborder=0></iframe>

The graph shows that the number of outages vary across regions, with the Northeast region having the most outages and the West North Central region having the least.

### Bivariate Analysis

I created a scatter plot that shows the relationship between number of customers affected and demand loss.

<iframe src="assets/bivariate.html" width=800 height=600 frameborder=0></iframe>

The graph shows that the majority of outages in the dataset affected a low number of customers and had a low demand loss. However, as the number of customers affected increases, it appears that there is also a slight increase in demand loss.

### Interesting Aggregates

I created a pivot table using the `CAUSE.CATEGORY` column and the `MONTH` column to look at the patterns in the number of power outages in each cause category that occurred in each month.

| CAUSE.CATEGORY                |   Jan |   Feb |   Mar |   Apr |   May |   Jun |   Jul |   Aug |   Sep |   Oct |   Nov |   Dec |
|:------------------------------|------:|------:|------:|------:|------:|------:|------:|------:|------:|------:|------:|------:|
| equipment failure             |     3 |     4 |     4 |     7 |     6 |     7 |    12 |     4 |     4 |   nan |     2 |     4 |
| fuel supply emergency         |     4 |     9 |     5 |     3 |     2 |     7 |     6 |     5 |     3 |   nan |     2 |     4 |
| intentional attack            |    50 |    40 |    42 |    43 |    46 |    34 |    34 |    26 |    21 |    34 |    24 |    24 |
| islanding                     |   nan |     6 |     3 |     2 |     5 |    10 |     4 |     4 |     3 |     3 |     3 |     3 |
| public appeal                 |     4 |     5 |     2 |   nan |     5 |    16 |    14 |    18 |     1 |     2 |   nan |     2 |
| severe weather                |    67 |    61 |    30 |    44 |    51 |   102 |    98 |    83 |    58 |    62 |    38 |    65 |
| system operability disruption |     8 |    11 |    14 |    12 |    12 |    19 |    13 |    13 |     4 |     8 |     3 |     9 |

The table shows some interesting patterns. For example, we can see that the number of outages for severe weather were abnormally high in June, July, and August. The same pattern exists for the category public appeal, where there were more outages caused by public appeal in June, July, and August. The pivot table helps with recognizing relationships between two features of the data.

---

## Assessment of Missingness
### NMAR Analysis

When data are NMAR (not missing at random), it means that the missingness of the data depends on the missing values themselves. One column that might be NMAR in this dataset is `IND.SALES`, which measures the industrial electricity consumption of a state. If `IND.SALES` was a low value, it might have been deemed negligible and not reported or included in the dataset. This would make the data NMAR because the missingness depends directly on the value of `IND.SALES`.

To explain the missingness of the data in the `IND.SALES` column, additional data could be collected. For example, the Industrial Production Index of each state at the time of each outage could be collected. If the outages with missing `IND.SALES` data also had low Industrial Production Indexes, the missingness of the data would be explained, thereby making it MAR (missing at random).

### Missingness Dependency

To continue the analysis of missingness in the dataset, I will explore the missingness of the `CUSTOMERS.AFFECTED` column and test whether it is dependent on the `CLIMATE.REGION` column. To do this, I will use a permutation test with the following hypotheses:

Null Hypothesis: The distribution of `CLIMATE.REGION` is the same when `CUSTOMERS.AFFECTED` is missing vs not missing.

Alternative Hypothesis: The distribution of `CLIMATE.REGION` is different when `CUSTOMERS.AFFECTED` is missing vs not missing.

The following plot shows the distributions of `CLIMATE.REGION` when `CUSTOMERS.AFFECTED` is missing vs not missing.

<iframe src="assets/climate-distribution.html" width=800 height=600 frameborder=0></iframe>

For this permutation test, I will use Total Variation Distance (TVD) as the test statistic because I am working with categorical variables. The observed TVD is 0.28. After running the permutation test and calculating 1000 more TVDs, it turns out that an observed TVD of 0.28 has a p-value of 0.0. A p-value of 0.0 means that I will reject the null hypothesis. This means that the distribution of `CLIMATE.REGION` is different when `CUSTOMERS.AFFECTED` is missing vs not missing, or that the missingness of `CUSTOMERS.AFFECTED` is MAR because it is dependent on `CLIMATE.REGION`.

Now, I will conduct another missingness dependency test. For the next test, I will look at the missingness of `CUSTOMERS.AFFECTED` and test whether it is dependent on `IND.CUSTOMERS`. This permutation test will use the following hypotheses:

Null Hypothesis: The distribution of `IND.CUSTOMERS` is the same when `CUSTOMERS.AFFECTED` is missing vs not missing.

Alternative Hypothesis: The distribution of `IND.CUSTOMERS` is different when `CUSTOMERS.AFFECTED` is missing vs not missing.

The following plot shows the distributions of `IND.CUSTOMERS` when `CUSTOMERS.AFFECTED` is missing vs not missing.

<iframe src="assets/ind-distribution.html" width=800 height=600 frameborder=0></iframe>

The distributions have roughly the same shape, so I will use absolute difference in means as the test statistic for this permutation test. The observed absolute difference in means is 1161.99. After running the permutation test and calculating 1000 more absolute differences, it turns out that an observed absolute difference in means of 1161.99 has a p-value of 0.58. A p-value greater than 0.05 means that I will fail to reject the null hypothesis. This means that the distribution of `IND.CUSTOMERS` is the same when `CUSTOMERS.AFFECTED` is missing vs not missing, or that the missingness of `CUSTOMERS.AFFECTED` is not dependent on `IND.CUSTOMERS`.

---

## Hypothesis Testing

For my hypothesis test, I will be testing whether the duration of power outages is the same for power outages that occur on the East Coast vs power outages that occur on the West Coast of the United States. To do this, I will use only the outages that occur in the West, Northwest, Northeast, and Southeast climate regions. I will create a new column based on these regions, containing the string 'West Coast' if the outage is in the West or Northwest climate region, and containing the value 'East Coast' if the outage is in the Northeast or Southeast climate region. Performing this hypothesis test will help me answer my original question (Which characteristics are associated with power outages in different regions of the U.S.?) because it will provide insight about outage duration, which is a characteristic of power outages, in different regions of the U.S. (west coast and east coast).

Null Hypothesis: The average duration of power outages that occur on the west coast is the same as the average duration of power outages that occur on the east coast

Alternative Hypothesis: The average duration of power outages that occur on the west coast is different than the average duration of power outages that occur on the east coast.

Test Statistic: Absolute difference in group means. Since I am specifically interested in the mean duration of power outages, the absolute difference in group means is the best test statistic for testing this hypothesis.

I will be using the standard significance level of 0.05 for this hypothesis test.

The observed absolute difference in group means is 1376.83. After running the permutation test and calculating 1000 more absolute differences, we can see that observed absolute difference in means of 1376.83 has a p-value of 0.0.

<iframe src="assets/hypothesis-test.html" width=800 height=600 frameborder=0></iframe>

Since the p-value of 0.0 is less than 0.05, I will reject the null hypothesis. The data supports the alternative hypothesis, which is that the average duration of power outages that occur on the west coast is different than the average duration of power outages that occur on the east coast. This is interesting because it implies that the severity of power outages is not the same across the country.

---

## Framing a Prediction Problem

I will create a model to predict the demand loss of a major power outage in megawatts. The model I create will be a regression model to predict the variable `DEMAND.LOSS.MW`. I chose to predict the demand lost in a power outage because demand loss has both economic and social implications. Larger demand loss results in larger economic loss and also means that the outage had an impact on a larger scale, so it would be useful to be able to anticipate the demand of a power outage. The metric I am using to evaluate my model is root mean squared error. I chose RMSE over other metrics like R squared because RMSE will provide the error in units that are easy to interpret in regards to the model.

The information that we would know at the time of prediction is `YEAR`, `MONTH`, `U.S._STATE`, `NERC.REGION`, `CLIMATE.REGION`, `ANOMALY.LEVEL`, `CLIMATE.CATEGORY`, `POPULATION`, `TOTAL.SALES`, `TOTAL.CUSTOMERS`, `POPPCT_URBAN`, `PC.REALGSP.STATE`, and `TOTAL.REALGSP`, so any of those features can be used to train the model.

---

## Baseline Model

The baseline model that I created uses the columns `POPPCT_URBAN` (quantitative), `ANOMALY.LEVEL` (quantitative), `TOTAL.CUSTOMERS` (quantitative), `CLIMATE.CATEGORY` (ordinal), and `NERC.REGION` (nominal). I encoded the ordinal `CLIMATE.CATEGORY` data by using a dictionary where colder climate categories correspond to higher numbers, so the data ended up as a series of integers representing the coldness of the climate category. I encoded the nominal `NERC.REGION` data using one hot encoding.

The baseline model achieved a RMSE of 1119.0 megawatts, which is okay considering the scale and units of the `DEMAND.LOSS.MW` data. The baseline model is not the best, but it is able to make predictions with some accuracy based on the parameters it was given.

---

## Final Model

For the final model, I added two new features to improve the model performance. The first new feature is based on the column `POPPCT_URBAN`, which contains the percentage of the total population in urban areas. I used a Binarizer to create a new feature that represents whether more than 75% of the population resides in an urban area, because if a large proportion of the population is clustered together, a power outage may cause more demand loss. This feature could help the model improve it's accuracy by providing information about the chances of a power outage being in an area where there are lots of people who demand large amounts of power.

Another new feature I created is based on the column `TOTAL.CUSTOMERS`. I used a QuantileTransformer to create a feature representing the quantiles of the total number of customers, which I designed to help to make more sense of the values in relation to one another. Outages that occur in states with larger quantiles of total customers seem more likely to have higher demand loss due to more customers losing power.

For the final model, I used LASSO Regression, which stands for Least Absolute Shrinkage and Selection Operator. It is similar to linear regression, but includes penalties to discourage sparse solutions and ignore redundant variables. I chose to tune the hyperparameters `alpha`, `positive`, `max_iter`, and `tol` using GridSearchCV. The hyperparameters that ended up performing the best were 1.0 for `alpha`, True for `positive`, 500 for `max_iter`, and 0.01 for `tol`.

The testing model achieved an RMSE of 1092.28, which is an improvement from the RMSE that was achieved by the baseline model. The final model, with new engineered features and hyperparameter tuning, does a better job of making predictions than the original baseline model.

---

## Fairness Analysis

In my fairness analysis, I will test whether the model performs differently when making predictions for power outages that occurred in states with a high percentage of the population living in urban areas vs power outages that occurred in states with a low percentage of the population living in urban areas. Outages that occurred in states that have a high percentage of the population living in urban areas are outages whose `POPPCT_URBAN` is greater than 80.

To perform this analysis, I will run a permutation test using a significance level of 0.05.

Null Hypothesis: The model is fair. It's root mean squared error for outages in highly urban states and less urban states are roughly the same, and any differences are due to random chance.

Alternative Hypothesis: The model is unfair. It's root mean squared error for outages in highly urban states and less urban states are different.

Test Statistic: I will use the absolute difference in RMSE as the statistic for this permutation test because it shows how similar the RMSE's are between the two groups.

The observed absolute difference in group RMSE is 726.76. After running the permutation test and calculating 1000 more absolute differences, it can be determined that observed absolute difference in RMSE of 726.76 has a p-value of 0.353.

<iframe src="assets/fairness-analysis.html" width=800 height=600 frameborder=0></iframe>

Since the p-value of 0.353 is greater than 0.05, I will fail to reject the null hypothesis. The data supports the null hypothesis saying that the model is fair when it makes predictions for the demand loss of a power outage, no matter if the outage occurred in a state with a high percentage of the population living in an urban area or a low percentage of the population living in an urban area.

---
