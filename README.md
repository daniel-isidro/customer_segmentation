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

We import the CSV and we put it into a dataframe with pandas. We know from domain knowledge that every row of the dataset is a different  order.

`df1.head()`

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

`df2.head()`

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

`df2.info()`

```
class 'pandas.core.frame.DataFrame';
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

`df2.describe()`

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

We see we have 235,574 transactions and 5 columns in the period of time included in the dataset. The biggest transaction was 150,000 units. But it seems there was a return of that amount as well, -150,000 units. The most expensive purchase was 2.41 Millions.

Let's view the period of time included in the dataset:

`df2['date'].min()`

```
Timestamp('2019-01-07 00:00:00')
```

`df2['date'].max()`

```
Timestamp('2020-11-30 00:00:00')
```

Let's explore in how many different countries we have sales in that period:

`df2['country'].unique()`

```
array(['KR', 'PK', 'MM', 'VN', 'IN', 'SA', 'PH', 'AF', 'CN', 'BD', 'ID',
       'TH', 'IQ', 'MY', 'JP', 'IR', 'TR', 'UZ'], dtype=object)
```

With the dataprep.clean package we can get the full country names:

`clean_country(df2, "country")['country_clean'].unique()`

```
array(['South Korea', 'Pakistan', 'Myanmar', 'Vietnam', 'India',
       'Saudi Arabia', 'Philippines', 'Afghanistan', 'China',
       'Bangladesh', 'Indonesia', 'Thailand', 'Iraq', 'Malaysia', 'Japan',
       'Iran', 'Turkey', 'Uzbekistan'], dtype=object)
```

Total number of customers in all countries:

`df2['id'].nunique()`

```
21837
```

# Data Exploration

For greater visibility in the plots we convert the dates to monthly periods:

`df2c = df2b.to_period("M")`

`df2c.head()`

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

`df2c['units'].groupby('date').agg(sum).plot(figsize=(20,5));`

<img src="https://raw.githubusercontent.com/daniel-isidro/customer_segmentation/main/media/units.png" width="800">

Revenue chart:

`df2c['monetary'].groupby('date').agg(sum).plot(figsize=(20,5));`

<img src="https://raw.githubusercontent.com/daniel-isidro/customer_segmentation/main/media/monetary.png" width="800">

# Modeling

## Transforming the data to obtain RFM values

We will transform the data to assign each customer some scores depending on the purchases they did. Prior to that, we will create some new features 'recency', 'frequency', as long as the previoulsy created 'monetary' feature.

**Recency** will be the minimum of 'days_since_last_purchase' for each customer.

**Frequency** will be the total number of orders in the period for each customer.

**Monetary**, will be the total value of the purchases in the period for each customer.

We will focus on sales from last 365 days since the most recent date.

**TIP:** There are customers with the same 'id' in several countries. This causes errors in the monetary values. We will solve this by creating a new feature: a unique 'id+' identifier that combines country code and customer id.

`df3['id+'] = df3['country'].map(str) + df3['id'].map(str)`

`df3.head()`

<table class="dataframe" border="0">
  <thead>
    <tr>
      <th></th>
      <th>country</th>
      <th>id</th>
      <th>monetary</th>
      <th>units</th>
      <th>date</th>
      <th>id+</th>
      <th>days_since_purchase</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>KR</td>
      <td>4375152</td>
      <td>773.58</td>
      <td>1</td>
      <td>2019-12-16</td>
      <td>KR4375152</td>
      <td>351</td>
    </tr>
    <tr>
      <th>1</th>
      <td>KR</td>
      <td>705462</td>
      <td>337.26</td>
      <td>1</td>
      <td>2019-12-09</td>
      <td>KR705462</td>
      <td>358</td>
    </tr>
    <tr>
      <th>2</th>
      <td>KR</td>
      <td>705462</td>
      <td>337.26</td>
      <td>1</td>
      <td>2019-12-23</td>
      <td>KR705462</td>
      <td>344</td>
    </tr>
    <tr>
      <th>3</th>
      <td>KR</td>
      <td>705462</td>
      <td>421.56</td>
      <td>2</td>
      <td>2019-12-16</td>
      <td>KR705462</td>
      <td>351</td>
    </tr>
    <tr>
      <th>4</th>
      <td>KR</td>
      <td>706854</td>
      <td>391.50</td>
      <td>1</td>
      <td>2019-12-09</td>
      <td>KR706854</td>
      <td>358</td>
    </tr>
  </tbody>
</table>

The result will be a dataframe that contains three new columns.

`rfm.head()`

<table class="dataframe" border="0">
  <thead>
    <tr>
      <th></th>
      <th>id</th>
      <th>id+</th>
      <th>country</th>
      <th>recency</th>
      <th>frequency</th>
      <th>monetary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>600018</td>
      <td>CN600018</td>
      <td>CN</td>
      <td>29</td>
      <td>7</td>
      <td>21402.78</td>
    </tr>
    <tr>
      <th>1</th>
      <td>600060</td>
      <td>CN600060</td>
      <td>CN</td>
      <td>155</td>
      <td>1</td>
      <td>1201.14</td>
    </tr>
    <tr>
      <th>2</th>
      <td>600462</td>
      <td>CN600462</td>
      <td>CN</td>
      <td>211</td>
      <td>2</td>
      <td>2033.64</td>
    </tr>
    <tr>
      <th>3</th>
      <td>600888</td>
      <td>CN600888</td>
      <td>CN</td>
      <td>8</td>
      <td>3</td>
      <td>2335.80</td>
    </tr>
    <tr>
      <th>4</th>
      <td>601014</td>
      <td>CN601014</td>
      <td>CN</td>
      <td>225</td>
      <td>1</td>
      <td>230.52</td>
    </tr>
  </tbody>
</table>


## Calculating the R, F and M scores

No we'll assign a rate between 1 and 5 depending on recency, monetary and frequency parameters. We'll use the quintiles method, dividing every feature on groups that contain 20 % of the samples.

Higher values are better for frequency and monetary, while lower values are better for recency. Those will be the asssigned R, F and M scores to each customer.

Then we concatenate R, F and M values to obtain a combined RFM score per customer.

`rfm.head()`

<table class="dataframe" border="0">
  <thead>
    <tr>
      <th></th>
      <th>id</th>
      <th>country</th>
      <th>recency</th>
      <th>frequency</th>
      <th>monetary</th>
      <th>r</th>
      <th>f</th>
      <th>m</th>
      <th>rfm_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>600018</td>
      <td>CN</td>
      <td>29</td>
      <td>7</td>
      <td>21402.78</td>
      <td>4</td>
      <td>4</td>
      <td>5</td>
      <td>445</td>
    </tr>
    <tr>
      <th>1</th>
      <td>600060</td>
      <td>CN</td>
      <td>155</td>
      <td>1</td>
      <td>1201.14</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>212</td>
    </tr>
    <tr>
      <th>2</th>
      <td>600462</td>
      <td>CN</td>
      <td>211</td>
      <td>2</td>
      <td>2033.64</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>222</td>
    </tr>
    <tr>
      <th>3</th>
      <td>600888</td>
      <td>CN</td>
      <td>8</td>
      <td>3</td>
      <td>2335.80</td>
      <td>5</td>
      <td>3</td>
      <td>3</td>
      <td>533</td>
    </tr>
    <tr>
      <th>4</th>
      <td>601014</td>
      <td>CN</td>
      <td>225</td>
      <td>1</td>
      <td>230.52</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>211</td>
    </tr>
  </tbody>
</table>

With this rfm scores we would have 125 segments of customers, which is too much for any practical analysis. To get a more simple segmentation, we choose to create a new feature 'fm' that combines 'f' and 'm' scores.

`def truncate(x):
    return math.trunc(x)`

`rfm['fm'] = ((rfm['f'] + rfm['m'])/2).apply(lambda x: truncate(x))`

<table class="dataframe" border="0">
  <thead>
    <tr>
      <th></th>
      <th>id</th>
      <th>country</th>
      <th>recency</th>
      <th>frequency</th>
      <th>monetary</th>
      <th>r</th>
      <th>f</th>
      <th>m</th>
      <th>rfm_score</th>
      <th>fm</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>600018</td>
      <td>CN</td>
      <td>29</td>
      <td>7</td>
      <td>21402.78</td>
      <td>4</td>
      <td>4</td>
      <td>5</td>
      <td>445</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>600060</td>
      <td>CN</td>
      <td>155</td>
      <td>1</td>
      <td>1201.14</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>212</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>600462</td>
      <td>CN</td>
      <td>211</td>
      <td>2</td>
      <td>2033.64</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>222</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>600888</td>
      <td>CN</td>
      <td>8</td>
      <td>3</td>
      <td>2335.80</td>
      <td>5</td>
      <td>3</td>
      <td>3</td>
      <td>533</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>601014</td>
      <td>CN</td>
      <td>225</td>
      <td>1</td>
      <td>230.52</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>211</td>
      <td>1</td>
    </tr>
  </tbody>
</table>

Then We create a segment map of only 11 segments based on only two scores, 'r' and 'fm'. We assign to each customer a different segment.

<table class="dataframe" border="0">
  <thead>
    <tr>
      <th></th>
      <th>id</th>
      <th>country</th>
      <th>recency</th>
      <th>frequency</th>
      <th>monetary</th>
      <th>r</th>
      <th>f</th>
      <th>m</th>
      <th>rfm_score</th>
      <th>fm</th>
      <th>segment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>600018</td>
      <td>CN</td>
      <td>29</td>
      <td>7</td>
      <td>21402.78</td>
      <td>4</td>
      <td>4</td>
      <td>5</td>
      <td>445</td>
      <td>4</td>
      <td>loyal customers</td>
    </tr>
    <tr>
      <th>1</th>
      <td>600060</td>
      <td>CN</td>
      <td>155</td>
      <td>1</td>
      <td>1201.14</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>212</td>
      <td>1</td>
      <td>lost</td>
    </tr>
    <tr>
      <th>2</th>
      <td>600462</td>
      <td>CN</td>
      <td>211</td>
      <td>2</td>
      <td>2033.64</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>222</td>
      <td>2</td>
      <td>hibernating</td>
    </tr>
    <tr>
      <th>3</th>
      <td>600888</td>
      <td>CN</td>
      <td>8</td>
      <td>3</td>
      <td>2335.80</td>
      <td>5</td>
      <td>3</td>
      <td>3</td>
      <td>533</td>
      <td>3</td>
      <td>potential loyalists</td>
    </tr>
    <tr>
      <th>4</th>
      <td>601014</td>
      <td>CN</td>
      <td>225</td>
      <td>1</td>
      <td>230.52</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>211</td>
      <td>1</td>
      <td>lost</td>
    </tr>
  </tbody>
</table>

## Segment description

* **Champions**	Bought recently, buy often and spend the most
* **Loyal Customers**	Buy on a regular basis. Responsive to promotions.
* **Potential Loyalists**	Recent customers with average frequency.
* **Recent Customers**	Bought most recently, but not often.
* **Promising**	Recent shoppers, but haven’t spent much.
* **Customers Needing Attention**	Above average recency, frequency and monetary values. May not have bought very recently though.
* **About To Sleep**	Below average recency and frequency. Will lose them if not reactivated.
* **At Risk**	Purchased often but a long time ago. Need to bring them back!
* **Can’t Lose Them**	Used to purchase frequently but haven’t returned for a long time.
* **Hibernating**	Last purchase was long back and low number of orders.
* **Lost** Purchased long time ago and never came back.

## Exploring the customer segments

We take a look at some segments.

**Can't lose**

`rfm[rfm['segment']=="can't lose"].sort_values(by='monetary', ascending=False)
`

<table class="dataframe" border="0">
  <thead>
    <tr>
      <th></th>
      <th>id</th>
      <th>country</th>
      <th>recency</th>
      <th>frequency</th>
      <th>monetary</th>
      <th>r</th>
      <th>f</th>
      <th>m</th>
      <th>rfm_score</th>
      <th>fm</th>
      <th>segment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>13028</th>
      <td>4096386</td>
      <td>JP</td>
      <td>260</td>
      <td>105</td>
      <td>220267.86</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
      <td>155</td>
      <td>5</td>
      <td>can't lose</td>
    </tr>
    <tr>
      <th>3502</th>
      <td>2443284</td>
      <td>IN</td>
      <td>246</td>
      <td>10</td>
      <td>102208.02</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
      <td>155</td>
      <td>5</td>
      <td>can't lose</td>
    </tr>
    <tr>
      <th>14174</th>
      <td>4262646</td>
      <td>IN</td>
      <td>316</td>
      <td>10</td>
      <td>91909.44</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
      <td>155</td>
      <td>5</td>
      <td>can't lose</td>
    </tr>
    <tr>
      <th>2435</th>
      <td>1803672</td>
      <td>IN</td>
      <td>267</td>
      <td>12</td>
      <td>70506.96</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
      <td>155</td>
      <td>5</td>
      <td>can't lose</td>
    </tr>
    <tr>
      <th>13254</th>
      <td>4132968</td>
      <td>VN</td>
      <td>253</td>
      <td>26</td>
      <td>42535.14</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
      <td>155</td>
      <td>5</td>
      <td>can't lose</td>
    </tr>
  </tbody>
</table>

**Loyal customers**

`rfm[rfm['segment']=='loyal customers'].sort_values(by='monetary', ascending=False).head()
`

<table class="dataframe" border="0">
  <thead>
    <tr>
      <th></th>
      <th>id</th>
      <th>country</th>
      <th>recency</th>
      <th>frequency</th>
      <th>monetary</th>
      <th>r</th>
      <th>f</th>
      <th>m</th>
      <th>rfm_score</th>
      <th>fm</th>
      <th>segment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>15420</th>
      <td>4422780</td>
      <td>TR</td>
      <td>92</td>
      <td>13</td>
      <td>2315341.14</td>
      <td>3</td>
      <td>5</td>
      <td>5</td>
      <td>355</td>
      <td>5</td>
      <td>loyal customers</td>
    </tr>
    <tr>
      <th>2882</th>
      <td>2030526</td>
      <td>JP</td>
      <td>22</td>
      <td>50</td>
      <td>1519339.86</td>
      <td>4</td>
      <td>5</td>
      <td>5</td>
      <td>455</td>
      <td>5</td>
      <td>loyal customers</td>
    </tr>
    <tr>
      <th>3220</th>
      <td>2182446</td>
      <td>JP</td>
      <td>29</td>
      <td>18</td>
      <td>1492057.68</td>
      <td>4</td>
      <td>5</td>
      <td>5</td>
      <td>455</td>
      <td>5</td>
      <td>loyal customers</td>
    </tr>
    <tr>
      <th>12660</th>
      <td>4041366</td>
      <td>PK</td>
      <td>50</td>
      <td>9</td>
      <td>736626.96</td>
      <td>4</td>
      <td>4</td>
      <td>5</td>
      <td>445</td>
      <td>4</td>
      <td>loyal customers</td>
    </tr>
    </tbody>
</table>

**Champions**

`rfm[rfm['segment']=='champions'].sort_values(by='monetary', ascending=False).head()`

<table class="dataframe" border="0">
  <thead>
    <tr>
      <th></th>
      <th>id</th>
      <th>country</th>
      <th>recency</th>
      <th>frequency</th>
      <th>monetary</th>
      <th>r</th>
      <th>f</th>
      <th>m</th>
      <th>rfm_score</th>
      <th>fm</th>
      <th>segment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>173</th>
      <td>638544</td>
      <td>CN</td>
      <td>1</td>
      <td>217</td>
      <td>21482332.56</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>555</td>
      <td>5</td>
      <td>champions</td>
    </tr>
    <tr>
      <th>15436</th>
      <td>4424580</td>
      <td>CN</td>
      <td>1</td>
      <td>104</td>
      <td>16912322.46</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>555</td>
      <td>5</td>
      <td>champions</td>
    </tr>
    <tr>
      <th>14754</th>
      <td>4341960</td>
      <td>TR</td>
      <td>1</td>
      <td>200</td>
      <td>16550997.90</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>555</td>
      <td>5</td>
      <td>champions</td>
    </tr>
    <tr>
      <th>11942</th>
      <td>3929094</td>
      <td>ID</td>
      <td>1</td>
      <td>470</td>
      <td>8748884.64</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>555</td>
      <td>5</td>
      <td>champions</td>
    </tr>
    <tr>
      <th>9626</th>
      <td>3520734</td>
      <td>JP</td>
      <td>1</td>
      <td>198</td>
      <td>6207519.96</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>555</td>
      <td>5</td>
      <td>champions</td>
    </tr>
  </tbody>
</table>

**Customers with monetary over the average that need attention**

`rfm[(rfm['monetary']>rfm['monetary'].mean()) & (rfm['segment']=='need attention')].sort_values(by='monetary', ascending=False).head()`

<table class="dataframe" border="0">
  <thead>
    <tr>
      <th></th>
      <th>id</th>
      <th>country</th>
      <th>recency</th>
      <th>frequency</th>
      <th>monetary</th>
      <th>r</th>
      <th>f</th>
      <th>m</th>
      <th>rfm_score</th>
      <th>fm</th>
      <th>segment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>8245</th>
      <td>3242664</td>
      <td>TR</td>
      <td>64</td>
      <td>1</td>
      <td>73823.58</td>
      <td>3</td>
      <td>1</td>
      <td>5</td>
      <td>315</td>
      <td>3</td>
      <td>need attention</td>
    </tr>
    <tr>
      <th>13065</th>
      <td>4107798</td>
      <td>JP</td>
      <td>120</td>
      <td>2</td>
      <td>67257.48</td>
      <td>3</td>
      <td>2</td>
      <td>5</td>
      <td>325</td>
      <td>3</td>
      <td>need attention</td>
    </tr>
    <tr>
      <th>9847</th>
      <td>3561900</td>
      <td>ID</td>
      <td>120</td>
      <td>1</td>
      <td>59700.00</td>
      <td>3</td>
      <td>1</td>
      <td>5</td>
      <td>315</td>
      <td>3</td>
      <td>need attention</td>
    </tr>
    <tr>
      <th>6626</th>
      <td>2921070</td>
      <td>ID</td>
      <td>71</td>
      <td>2</td>
      <td>34730.22</td>
      <td>3</td>
      <td>2</td>
      <td>5</td>
      <td>325</td>
      <td>3</td>
      <td>need attention</td>
    </tr>
    <tr>
      <th>10009</th>
      <td>3587772</td>
      <td>CN</td>
      <td>92</td>
      <td>1</td>
      <td>29961.00</td>
      <td>3</td>
      <td>1</td>
      <td>5</td>
      <td>315</td>
      <td>3</td>
      <td>need attention</td>
    </tr>
  </tbody>
</table>

Let's do a scatter plot to explore the distribution of customers, with 'monetary' on the x-axis. Using a colormap for the 'm' score, we see that the majority of customers who spend the most also purchase more frequently.

![Scatterplot](https://raw.githubusercontent.com/daniel-isidro/customer_segmentation/main/media/scatter.png)

Finally we export the dataframe to a CSV file for later processing it in Power BI.

`rfm.to_csv('rfm_asia.csv', encoding='utf-8', index=False, float_format='%.2f')`

# Summary



# Front-end

The front-end of this project consists in a **Power BI dashboard** that processes the CSV resulting from executing the previous Python code, and visualizes the results in a pleasing dashboard. That dashboard then can be shared with multiple colleagues in the different countries of the company. You can access that file [here](https://github.com/daniel-isidro/customer_segmentation/blob/main/output/customer_segmentation.pbix) for exploring it.

![Customer Segmentation](https://github.com/daniel-isidro/customer_segmentation/blob/main/media/customer_segmentation.png?raw=true)

![RFM Analysis](https://github.com/daniel-isidro/customer_segmentation/blob/main/media/rfm_analysis.png?raw=true)

We observe that a lot of our customers (32 %) are **lost** or **hibernating** (they have a few orders from long ago). However, 28 % of our customers are either **champions** or **loyal customers**, meaning they spend the most and frequently. A segment to take care is **can't lose**, second segment with highest revenue, comprised of customers who used to spend a lot but have not returned in a while.

### User Manual

We can share the dashboard with our country sales managers so they can be guided to the customer id's on the different segments and take specific actions on them. Let's view an example: my Chinese colleague needs to be guided on what customers to contact next to **improve customer retention** or to **increase sales**. They would just need to open this dashboard, click on country (top right) and then on customer segment **need attention** or at **risk**, to have a specific list of customers, ordered by revenue spent in the last 365 days. (An unedited dataset would also include customer names and contact details, which in this case have been omitted for privacy reasons).

![Country - Need Attention](https://github.com/daniel-isidro/customer_segmentation/blob/main/media/cs_cn_need_attention.png?raw=true)

![Country - At Risk](https://github.com/daniel-isidro/customer_segmentation/blob/main/media/cs_cn_at_risk.png?raw=true)

A **reference guide** to the 11 segments and suggested actions per segment is included in the last tab of the dashboard.

![Reference](https://github.com/daniel-isidro/customer_segmentation/blob/main/media/reference.png?raw=true)





# Conclusions



# References

[**Guillaume Martin** - RFM Segmentation with Python](https://guillaume-martin.github.io/rfm-segmentation-with-python.html)

[**Google Cloud** - Predicting Customer Lifetime Value with AI Platform](https://cloud.google.com/architecture/clv-prediction-with-offline-training-intro#overview)

# About Me

[Daniel Isidro](https://www.linkedin.com/in/daniel-isidro/)
