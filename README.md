# Analyzing-Crime-in-Los-Angeles

## Overview
This is an EDA project that uses a modified version of [Los Angeles Open Data](https://data.lacity.org/) provided by [DataCamp](https://data.lacity.org/). The goal is to provide miningfull insights to the Los Angeles Police Department by analyzing crime data.

## Packages Required

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

## Data exploration
We start by loading the crimes.csv file as a DataFrame using pandas.

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

## Q1
Now, we want to calculate the hour during which most crimes are committed. Since the time of occurrence is stored as an object, we extract the first two characters (representing the hour) using slicing and convert them to integers. The most frequent hour is stored in the variable peak_crime_hour.

```python
crimes.describe()
crime_hours = crimes["TIME OCC"].str.slice(0,2).astype(int)
peak_crime_hour = crime_hours.value_counts().idxmax()
print(peak_crime_hour)
```

## Q2
To determine the most dangerous location between 10 PM and 4 AM, we filter the data to include only crimes that occurred during this time range and count the frequency of each location. The variable peak_night_crime_location stores the location with the highest number of crimes during these hours.

```python
crimes["crime_hours"] = crime_hours
crimes_committed_10_4 = crimes[(crimes["crime_hours"] >= 22) | (crimes["crime_hours"] < 4)]
peak_night_crime_location = crimes_committed_10_4["AREA NAME"].value_counts().idxmax()
print(peak_night_crime_location)
```

## Q3
Finally, we split the ages of the victims into seven bins. Then, we count the frequency in each bin to identify the age range with the highest number of crime victims.

```python
bins = [0,17,25,34,44,54,64,200]
labels = ["0-17", "18-25", "26-34", "35-44", "45-54", "55-64", "65+"]
crimes["Age Group"] = pd.cut(crimes["Vict Age"], bins = bins, labels = labels, right = True)
victim_ages_frequencies = crimes["Age Group"].value_counts()
print(victim_ages_frequencies)
victim_ages = victim_ages_frequencies
```

