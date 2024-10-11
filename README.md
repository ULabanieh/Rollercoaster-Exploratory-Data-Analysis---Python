# 🎢 Project Overview
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
![1446-0~1](https://github.com/user-attachments/assets/b1c342c6-3c82-45c6-9cbf-fa28614ecf67)

```python
df.head(5)
```
![1446-0~2](https://github.com/user-attachments/assets/1b0e7dc5-87c3-491e-a2a9-a3b99d7c842d)

```python
df.columns
```

![1446-0~3](https://github.com/user-attachments/assets/dfbc823e-b9d0-41b4-9b41-7893f9ab57ce)

```python
df.dtypes
```

![1446-0~4](https://github.com/user-attachments/assets/679402c2-6ace-4f48-bf1d-eb05a62bfb8b)

```python
df.describe()
```

![148112~1](https://github.com/user-attachments/assets/8a219a19-0ab0-4e8f-8cef-dd99dd714eed)


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

![1433D9~1](https://github.com/user-attachments/assets/537a2642-9099-4199-b09a-fc83991771b8)


### Correcting dtypes
---

```python
df['opening_date_clean'] = pd.to_datetime(df['opening_date_clean'])

df.dtypes
```

![14A3BE~1](https://github.com/user-attachments/assets/1e367e94-a248-445d-991b-2faaac9a0a48)


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

![1404A2~1](https://github.com/user-attachments/assets/47281e4c-e468-441c-a426-985580bf54f8)


### Checking for NA and duplicate values
---

```python
df.isna().sum()
```

![141EE3~1](https://github.com/user-attachments/assets/2fa0cd63-a390-4850-b7eb-9346db6a181c)


```python
df.loc[df.duplicated()]
```

![1446-02-17 12_42_49-Rollercoaster Dataset - Python - Main Vault - Obsidian v1 6 7](https://github.com/user-attachments/assets/aba8242e-a3d8-46c0-8313-8949ef9320b6)

```python
# Check for duplicate coaster name

df.loc[df.duplicated(subset = ['Coaster_Name'])]
```

![143020~1](https://github.com/user-attachments/assets/18cea060-aac7-4b67-90f2-5434ae9b9628)


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

![__results___23_1](https://github.com/user-attachments/assets/efa3958c-f436-4163-89bd-adc19e1e24ee)


## Rollercoaster Speed (mph)
---

```python
ax = df['Speed_mph'].plot(kind = 'hist', bins = 20, title = 'RCoaster Speed (mph)')

  

ax.set_xlabel('Speed(mph)')
```

![__results___24_1](https://github.com/user-attachments/assets/1e8b0958-beac-459b-b528-7899b57b466f)


```python
ax = df['Speed_mph'].plot(kind = 'kde',

                          title = 'RCoaster Speed (mph)')

ax.set_xlabel('Speed(mph)')
```

![__results___25_1](https://github.com/user-attachments/assets/fb377110-28c9-4ec0-a0c4-072d3a8e3090)


# Feature Relationships
---

## Rollercoaster Speed vs Height
---
```python
df.plot(kind = 'scatter', x='Speed_mph', y = 'Height_ft', title = 'RCoaster Speed vs. Height')

plt.show()
```

![__results___28_0](https://github.com/user-attachments/assets/906a478a-a44c-4ccc-8474-a40b6ea034fd)


```python
ax = sns.scatterplot(x='Speed_mph',
                y='Height_ft',
                hue='Year_Introduced',
                data=df)
ax.set_title('Coaster Speed vs. Height')
plt.show()
```

![__results___29_0](https://github.com/user-attachments/assets/3a7a03d2-9672-4910-8db5-97ed46472a62)


## All Features
---

```python
sns.pairplot(df,
             vars=['Year_Introduced','Speed_mph',
                   'Height_ft','Inversions','Gforce'],
            hue='Type_Main')
plt.show()
```

![__results___30_0](https://github.com/user-attachments/assets/36370015-60b6-4843-91ba-e6a863becc4c)


```python
df_corr = df[['Year_Introduced','Speed_mph',
    'Height_ft','Inversions','Gforce']].dropna().corr()
df_corr
```

![1446-02-17 12_51_49-🎢 Introduction to Exploratory Data Analysis - Brave](https://github.com/user-attachments/assets/e1d713f3-c95a-43f7-9db9-958770d9bc3e)


```python
sns.heatmap(df_corr, annot = True)
```

![__results___32_1](https://github.com/user-attachments/assets/469b823a-d0a1-4eae-acfc-579a84c52c51)


# Ask a question about the data
---
## What are the locations with the fastest rollercoasters (min. 10)
---

```python
df['Location'].value_counts()
```

![14DD3C~1](https://github.com/user-attachments/assets/88d6a12a-c936-4d6c-9a1c-78de7867d988)


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

![__results___34_0](https://github.com/user-attachments/assets/2d383eb6-82a1-404f-beb1-034cec2acf2a)
