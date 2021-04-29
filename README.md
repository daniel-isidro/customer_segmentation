# Customer Segmentation Analysis with RFM, using Python and Power BI

# Table of Contents

[Introduction](#introduction) <br>
[Methodology](#methodology) <br>
[Requirements](#requirements) <br>
[Execution Guide](#execution-Guide) <br>
[Data Acquisition](#data-acquisition) <br>
[Data Preparation](#data-preparation) <br>
[Raw Data Description](#raw-data-description) <br>
[Data Exploration](#data-exploration) <br>
[Modeling](#modeling) <br>
[Summary](#summary) <br>
[Front-end](#front-end) <br>
[Conclusions](#conclusions) <br>
[References](#references) <br>
[About Me](#about-me)

# Introduction



# Methodology



# Requirements

We'll use the [Google Colaboratory](https://colab.research.google.com/) Jupyter notebook environment (free), with Python 3.7 or higher, and [Microsoft Power BI Desktop](https://powerbi.microsoft.com/en-us/downloads/) application (free download, Windows only).

### Python packages

* numpy
* pandas
* math
* datetime
* dataprep
* matplotlib

# Execution Guide

For replicating the project, please execute the following steps in order:

1. **[Jupyter notebook](https://github.com/daniel-isidro/customer_segmentation/blob/main/code/customer_segmentation_asia.ipynb)**. Running this notebook the data will be explored, prepared, and the RFM segmentation will be performed. The output will be a [CSV file](https://github.com/daniel-isidro/customer_segmentation/blob/main/output/rfm_asia.csv).

2. **[Power BI file](https://github.com/daniel-isidro/customer_segmentation/blob/main/output/customer_segmentation.pbix)**. This is a file that processes the CSV file obtained in the previous step and presents it as a dashboard so it can be leveraged for business actions in the countries.

# Data Acquisition

Data has been obtained from **real sales orders** in 18 countries in a period of around 2 years. Some features (country names, customer id and revenue), have been altered in order to preserve privacy. Number of orders, units and dates have not been modified. It is acquired as a CSV file.

# Data Preparation

We know from domain knowledge that every row of the dataset is a different  order.

```df1.head()```

<table class="dataframe" border="0">
  <thead>
    <tr>
      <th></th>
      <th>country</th>
      <th>id</th>
      <th>week.year</th>
      <th>revenue</th>
      <th>units</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>KR</td>
      <td>702234</td>
      <td>03.2019</td>
      <td>808.08</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>KR</td>
      <td>702234</td>
      <td>06.2019</td>
      <td>1606.80</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>KR</td>
      <td>3618438</td>
      <td>08.2019</td>
      <td>803.40</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>KR</td>
      <td>3618438</td>
      <td>09.2019</td>
      <td>803.40</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>KR</td>
      <td>3618438</td>
      <td>09.2019</td>
      <td>803.40</td>
      <td>1</td>
    </tr>
  </tbody>
</table>

Also, we notice that the date is expressed as week of the year, so for better analysis we convert it to year-month-day format with the datetime package. Also we rename 'revenue' to 'monetary' per convention in the RFM analysis.

```df2.head()```

<table class="dataframe" border="0">
  <thead>
    <tr>
      <th></th>
      <th>country</th>
      <th>id</th>
      <th>monetary</th>
      <th>units</th>
      <th>date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>KR</td>
      <td>702234</td>
      <td>808.08</td>
      <td>1</td>
      <td>2019-01-21</td>
    </tr>
    <tr>
      <th>1</th>
      <td>KR</td>
      <td>702234</td>
      <td>1606.80</td>
      <td>2</td>
      <td>2019-02-11</td>
    </tr>
    <tr>
      <th>2</th>
      <td>KR</td>
      <td>3618438</td>
      <td>803.40</td>
      <td>1</td>
      <td>2019-02-25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>KR</td>
      <td>3618438</td>
      <td>803.40</td>
      <td>1</td>
      <td>2019-03-04</td>
    </tr>
    <tr>
      <th>4</th>
      <td>KR</td>
      <td>3618438</td>
      <td>803.40</td>
      <td>1</td>
      <td>2019-03-04</td>
    </tr>
  </tbody>
</table>

# Raw Data Description

```df2.info()```

```
class 'pandas.core.frame.DataFrame'&gt;
RangeIndex: 235574 entries, 0 to 235573
Data columns (total 5 columns):
 #   Column    Non-Null Count   Dtype         
---  ------    --------------   -----         
 0   country   235574 non-null  object        
 1   id        235574 non-null  int64         
 2   monetary  235574 non-null  float64       
 3   units     235574 non-null  int64         
 4   date      235574 non-null  datetime64[ns]
dtypes: datetime64[ns](1), float64(1), int64(2), object(1)
memory usage: 9.0+ MB
```

```df2.describe()```

<table class="dataframe" border="0">
  <thead>
    <tr>
      <th></th>
      <th>id</th>
      <th>monetary</th>
      <th>units</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>2.355740e+05</td>
      <td>2.355740e+05</td>
      <td>235574.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>3.193118e+06</td>
      <td>2.840211e+03</td>
      <td>8.599642</td>
    </tr>
    <tr>
      <th>std</th>
      <td>7.371744e+06</td>
      <td>2.247532e+04</td>
      <td>602.939290</td>
    </tr>
    <tr>
      <th>min</th>
      <td>6.000180e+05</td>
      <td>-1.061539e+05</td>
      <td>-150000.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2.214396e+06</td>
      <td>3.994800e+02</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>3.140856e+06</td>
      <td>1.150320e+03</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>3.892650e+06</td>
      <td>2.216160e+03</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2.419308e+08</td>
      <td>2.415857e+06</td>
      <td>150000.000000</td>
    </tr>
  </tbody>
</table>

We see we have 235,574 transactions in the period of time included in the dataset. The biggest transaction was 150,000 units. But it seems there was a return of that amount as well, -150,000 units. The most expensive purchase was 2.41 Millions.

Let's view the period of time included in the dataset:

```df2['date'].min()```

```
Timestamp('2019-01-07 00:00:00')
```

```df2['date'].max()```

```
Timestamp('2020-11-30 00:00:00')
```

Let's explore in how many different countries we have sales in that period:

```df2['country'].unique()```

```
array(['KR', 'PK', 'MM', 'VN', 'IN', 'SA', 'PH', 'AF', 'CN', 'BD', 'ID',
       'TH', 'IQ', 'MY', 'JP', 'IR', 'TR', 'UZ'], dtype=object)
```

With the dataprep.clean package we can get the full country names:

```clean_country(df2, "country")['country_clean'].unique()```

```
array(['South Korea', 'Pakistan', 'Myanmar', 'Vietnam', 'India',
       'Saudi Arabia', 'Philippines', 'Afghanistan', 'China',
       'Bangladesh', 'Indonesia', 'Thailand', 'Iraq', 'Malaysia', 'Japan',
       'Iran', 'Turkey', 'Uzbekistan'], dtype=object)
```

Total number of customers in all countries:

```df2['id'].nunique()```

```
21837
```

# Data Exploration

For greater visibility in the plots we convert the dates to monthly periods:

```df2c = df2b.to_period("M")```

```df2c.head()```

<table class="dataframe" border="0">
  <thead>
    <tr>
      <th></th>
      <th>country</th>
      <th>id</th>
      <th>monetary</th>
      <th>units</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-01</th>
      <td>KR</td>
      <td>702234</td>
      <td>808.08</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2019-02</th>
      <td>KR</td>
      <td>702234</td>
      <td>1606.80</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2019-02</th>
      <td>KR</td>
      <td>3618438</td>
      <td>803.40</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2019-03</th>
      <td>KR</td>
      <td>3618438</td>
      <td>803.40</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2019-03</th>
      <td>KR</td>
      <td>3618438</td>
      <td>803.40</td>
      <td>1</td>
    </tr>
  </tbody>
</table>

We aggregate the units and revenue of the same period.

Units chart:

```df2c['units'].groupby('date').agg(sum).plot(figsize=(20,5));```

<img src="https://raw.githubusercontent.com/daniel-isidro/customer_segmentation/main/media/units.png" width="800">

Revenue chart:

```df2c['monetary'].groupby('date').agg(sum).plot(figsize=(20,5));```

<img src="https://raw.githubusercontent.com/daniel-isidro/customer_segmentation/main/media/monetary.png" width="800">

# Modeling



# Summary



# Front-end



### User Manual



# Conclusions



# References

[**Guillaume Martin** - RFM Segmentation with Python](https://guillaume-martin.github.io/rfm-segmentation-with-python.html)

[**Google Cloud** - Predicting Customer Lifetime Value with AI Platform](https://cloud.google.com/architecture/clv-prediction-with-offline-training-intro#overview)

# About Me

[Daniel Isidro](https://www.linkedin.com/in/daniel-isidro/)
