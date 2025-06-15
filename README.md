# Analyzing Crime in Los Angeles

## Overview
This is an EDA project that uses a modified version of [Los Angeles Open Data](https://data.lacity.org/). The project Analyzing Crime in Los Angeles and the modified data are provided by [DataCamp](https://data.lacity.org/). The goal is to provide miningfull insights to the Los Angeles Police Department by analyzing crime data.

## Packages Required

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

## Data exploration
We begin by loading the crimes.csv file as a DataFrame using pandas.

```python
crimes = pd.read_csv("crimes.csv", dtype={"TIME OCC": str})
crimes.head()
```


Next, we inspect the DataFrame for missing values and verify the data types of all columns.

```python
print(crimes.shape)
crimes.isnull().sum()
crimes.info()
```
(185715, 12)
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 185715 entries, 0 to 185714
Data columns (total 12 columns):
| #  | Column        | Non-Null Count | Dtype  |
|----|---------------|----------------|--------|
| 0  | DR_NO         | 185715         | int64  |
| 1  | Date Rptd     | 185715         | object |
| 2  | DATE OCC      | 185715         | object |
| 3  | TIME OCC      | 185715         | object |
| 4  | AREA NAME     | 185715         | object |
| 5  | Crm Cd Desc   | 185715         | object |
| 6  | Vict Age      | 185715         | int64  |
| 7  | Vict Sex      | 185704         | object |
| 8  | Vict Descent  | 185705         | object |
| 9  | Weapon Desc   | 73502          | object |
| 10 | Status Desc   | 185715         | object |
| 11 | LOCATION      | 185715         | object |
dtypes: int64(2), object(10)
memory usage: 17.0+ MB

## Peak hour crime
Now, we want to calculate the hour during which most crimes are committed. Since the time of occurrence is stored as an object, we extract the first two characters (representing the hour) using slicing and convert them to integers. The most frequent hour is stored in the variable peak_crime_hour.

```python
crimes.describe()
crime_hours = crimes["TIME OCC"].str.slice(0,2).astype(int)
peak_crime_hour = crime_hours.value_counts().idxmax()
print("peak hour crime =", peak_crime_hour)
```
peak hour crime = 12

## Peak night crime location
To determine the most dangerous location between 10 PM and 4 AM, we filter the data to include only crimes that occurred during this time range and count the frequency of each location. The variable peak_night_crime_location stores the location with the highest number of crimes during these hours.

```python
crimes["crime_hours"] = crime_hours
crimes_committed_10_4 = crimes[(crimes["crime_hours"] >= 22) | (crimes["crime_hours"] < 4)]
peak_night_crime_location = crimes_committed_10_4["AREA NAME"].value_counts().idxmax()
print("peak night crime location =", peak_night_crime_location)
```
peak night crime location = Central

## Victim ages frequencies
Finally, we split the ages of the victims into seven bins. Then, we count the frequency in each bin to identify the age range with the highest number of crime victims.

```python
bins = [0,17,25,34,44,54,64,200]
labels = ["0-17", "18-25", "26-34", "35-44", "45-54", "55-64", "65+"]
crimes["Age Group"] = pd.cut(crimes["Vict Age"], bins = bins, labels = labels, right = True)
victim_ages_frequencies = crimes["Age Group"].value_counts()
print(victim_ages_frequencies)
victim_ages = victim_ages_frequencies
```
26-34    47470
35-44    42157
45-54    28353
18-25    28291
55-64    20169
65+      14747
0-17      4528
Name: Age Group, dtype: int64


