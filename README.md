# Project Overview
---
This is a Data Exploration, Data Cleaning and Data Visualization project made using Python and the Pandas library about a rollercoaster dataset where I looked into some interesting insights such as years with the highest number of Rollercoasters introduced, highest speeds and heights, highest speeds by location and more. 

The dataset contains information about over 1000 rollercoasters and the information was scraped from Wikipedia.

This project was completed in a Jupyter Notebook which you may download and review at will. I will outline below an overview of the steps I took towards completing this project, but for full detail download the project files.
# Import Data
---
First of all, I imported the libraries I will be using in this project along with some settings for visual and dataframe formatting. To import the data, I downloaded the dataset in CSV format from Kaggle ([Roller Coaster Database](https://www.kaggle.com/datasets/robikscube/rollercoaster-database)) and read it in using the `pd.read_csv()` method.

```python
import pandas as pd

import numpy as np

import matplotlib.pylab as plt

import seaborn as sns

plt.style.use('ggplot')

pd.set_option('display.max_columns', 200)
```

```python
df = pd.read_csv ('/kaggle/input/rollercoaster/coaster_db.csv')
```


# Understanding Data
---
Take a birds eye view look at the data, such as how many rows and columns it contains, what kind of information is included in the dataset, data types, and statistical summary metrics. Here is an overview of the code along with outputs:

```python
df.shape
```

![[1446-02-17 11_40_37-rollercoasterexploratory-data-analysis-with-python.ipynb - Visual Studio Code.png]]

```python
df.head(5)
```

![[1446-02-17 11_46_01-● rollercoasterexploratory-data-analysis-with-python.ipynb - Visual Studio Code.png]]

```python
df.columns
```

![[1446-02-17 11_46_42-● rollercoasterexploratory-data-analysis-with-python.ipynb - Visual Studio Code.png]]

```python
df.dtypes
```

![[1446-02-17 11_47_11-● rollercoasterexploratory-data-analysis-with-python.ipynb - Visual Studio Code.png]]

```python
df.describe()
```

![[1446-02-17 11_47_51-● rollercoasterexploratory-data-analysis-with-python.ipynb - Visual Studio Code.png]]

# Data Cleaning
---
Took a few data cleaning steps to prepare the dataframe for further analysis and visualization. Here are the main steps I took: 

### Dropping irrelevant columns and rows
---

```python
df = df[['coaster_name',

        #'Length', 'Speed',

            'Location', 'Status',

       # 'Opening date',

       #'Type',

        'Manufacturer',

        #'Height restriction', 'Model', 'Height',

       # 'Inversions', 'Lift/launch system', 'Cost', 'Trains', 'Park section',

       # 'Duration', 'Capacity', 'G-force', 'Designer', 'Max vertical angle',

       # 'Drop', 'Soft opening date', 'Fast Lane available', 'Replaced',

       # 'Track layout', 'Fastrack available', 'Soft opening date.1',

       # 'Closing date', 'Opened', 'Replaced by', 'Website',

       # 'Flash Pass Available', 'Must transfer from wheelchair', 'Theme',

       #'Single rider line available', 'Restraint Style',

       # 'Flash Pass available', 'Acceleration', 'Restraints', 'Name',

       'year_introduced', 'latitude', 'longitude', 'Type_Main',

       'opening_date_clean',

         #'speed1', 'speed2', 'speed1_value', 'speed1_unit',

       'speed_mph',

        #'height_value', 'height_unit',

        'height_ft',

       #'Inversions_clean',

        'Gforce_clean']].copy()

df
```

![[1446-02-17 12_36_26-● rollercoasterexploratory-data-analysis-with-python.ipynb - Visual Studio Code.png]]

### Correcting dtypes
---

```python
df['opening_date_clean'] = pd.to_datetime(df['opening_date_clean'])

df.dtypes
```


![[1446-02-17 12_38_42-● rollercoasterexploratory-data-analysis-with-python.ipynb - Visual Studio Code.png]]

### Renaming columns
---

```python
# Rename Columns

df = df.rename(columns={'coaster_name':'Coaster_Name',

                   'year_introduced':'Year_Introduced',

                    'latitude':'Latitude',

                    'longitude':'Longitude',

                   'opening_date_clean':'Opening_Date',

                   'speed_mph':'Speed_mph',

                   'height_ft':'Height_ft',

                   'Inversions_clean':'Inversions',

                   'Gforce_clean':'Gforce'})

df
```


![[1446-02-17 12_39_37-● rollercoasterexploratory-data-analysis-with-python.ipynb - Visual Studio Code.png]]
### Checking for NA and duplicate values
---

```python
df.isna().sum()
```


![[1446-02-17 12_42_08-● rollercoasterexploratory-data-analysis-with-python.ipynb - Visual Studio Code.png]]

```python
df.loc[df.duplicated()]
```

![[1446-02-17 12_42_49-Rollercoaster Dataset - Python - Main Vault - Obsidian v1.6.7.png]]

```python
# Check for duplicate coaster name

df.loc[df.duplicated(subset = ['Coaster_Name'])]
```

![[1446-02-17 12_43_15-● rollercoasterexploratory-data-analysis-with-python.ipynb - Visual Studio Code.png]]

```python
# removing duplicates
df = df.loc[~df.duplicated(subset = ['Coaster_Name', 'Location', 'Opening_Date'])] \

    .reset_index(drop = True).copy()
```

# Feature Understanding
---

## Top Years by Rollercoasters introduce
---

```python
ax = df['Year_Introduced'].value_counts() \

    .head(10) \

    .plot(kind = 'bar', title = 'Top Years by RCoasters Introduced')

ax.set_xlabel('Year Introduced')

ax.set_ylabel('Count')
```

![[__results___23_1.png]]


## Rollercoaster Speed (mph)
---

```python
ax = df['Speed_mph'].plot(kind = 'hist', bins = 20, title = 'RCoaster Speed (mph)')

  

ax.set_xlabel('Speed(mph)')
```

![[__results___24_1.png]]

```python
ax = df['Speed_mph'].plot(kind = 'kde',

                          title = 'RCoaster Speed (mph)')

ax.set_xlabel('Speed(mph)')
```

![[__results___25_1.png]]

# Feature Relationships
---

## Rollercoaster Speed vs Height
---
```python
df.plot(kind = 'scatter', x='Speed_mph', y = 'Height_ft', title = 'RCoaster Speed vs. Height')

plt.show()
```

![[__results___28_0.png]]

```python
ax = sns.scatterplot(x='Speed_mph',
                y='Height_ft',
                hue='Year_Introduced',
                data=df)
ax.set_title('Coaster Speed vs. Height')
plt.show()
```

![[__results___29_0.png]]

## All Features
---

```python
sns.pairplot(df,
             vars=['Year_Introduced','Speed_mph',
                   'Height_ft','Inversions','Gforce'],
            hue='Type_Main')
plt.show()
```

![[__results___30_0.png]]

```python
df_corr = df[['Year_Introduced','Speed_mph',
    'Height_ft','Inversions','Gforce']].dropna().corr()
df_corr
```


![[1446-02-17 12_51_49-🎢 Introduction to Exploratory Data Analysis - Brave.png]]

```python
sns.heatmap(df_corr, annot = True)
```

![[__results___32_1.png]]
# Ask a question about the data
---
## What are the locations with the fastest rollercoasters (min. 10)
---

```python
df['Location'].value_counts()
```

![[1446-02-17 12_53_16-● rollercoasterexploratory-data-analysis-with-python.ipynb - Visual Studio Code.png]]

```python
ax = df.query('Location != "Other"' ) \

    .groupby('Location')['Speed_mph'] \

    .agg(['mean', 'count']) \

    .query('count >= 10') \

    .sort_values('mean')['mean'] \

    .plot(kind = 'barh', figsize = (12,5), title = 'Average RCoaster Speed by Location')

ax.set_xlabel('Average RCoaster Speed')

plt.show()
```

![[__results___34_0.png]]
