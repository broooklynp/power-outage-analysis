# Analysis of Major Power Outages in the U.S.

by Brooklyn Pagaza

---

## Introduction

The dataset used for this analysis contains data about major power outages that occurred in the continental U.S. from January 2000 to July 2016. It includes one row for every observed major power outage, and contains information for each outage including the location, cause, date, and duration of the outage. In this analysis, I will be exploring the question: Which characteristics are associated with power outages in different regions of the U.S.?

Knowing about the causes of power outages across the United States can help citizens and the government decrease the likelihood of power outages occurring in the future. If there are observable patterns that point to a power outage occurring, action can be taken ahead of time to prevent a major power outage. Importantly, the characteristics that correspond with a major power outage might be different in different regions of the U.S. due to factors like weather, geography, or population, so it is important to understand those trends in order to be prepared. Since power is so essential to our lives today, it makes sense that we should use the information available to try to prevent major power outages.

The raw dataset used for this analysis contains 1534 rows. I will be using the following columns in my analysis:

|<div align="center">Column</div>|<div align="center">Description</div> |
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

The first few rows of the cleaned dataframe are shown below.

print(data.head().to_markdown(index=False))


---

## Assessment of Missingness

text

---

## Hypothesis Testing

text

---

## Framing a Prediction Problem

text

---

## Baseline Model

text

---

## Final Model

text

---

## Fairness Analysis

text

---
