# Cohort-Analysis-for-assessing-customer-retention-in-E-commerce-industry
![patrick-tomasso-fMntI8HAAB8-unsplash](https://github.com/user-attachments/assets/a67899f3-3b20-4cf4-8b25-68e5272b5229)

## Table Of Content
- [Overview](#overview)
- [Focus Areas](#focus-areas)
- [Business Overview/Problem](#business-overview/problem)
- [Rationale for the Project](#rationale-for-the-Project)
- [Data Description](#data-description)
- [Tech Stack](#tech-stack)
- [Libraries](#libraries)
- [Code Section](#code-section)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Analysis By Country](#analysis-by-country)
- [Sales Trend](#sales-trend)
- [Cohort Analysis](#cohort-analysis)
- [Observations](#observations)
- [Recommendations](#recommendations)
- [Average quantity bought](#average-quantity-bought)
- [Observations](#observations)
- [Recommendation](#recommendation)
## Overview

In this project, I delved into Cohort Analysis to gain a deeper understanding of customer behavior. This analytical approach enabled me to segment customers into cohorts based on their purchase behavior over time, which was instrumental in identifying key retention opportunities and optimizing marketing efforts. By analyzing these cohorts, I developed strategies that helped the company's management improve customer engagement and increase the effectiveness of their marketing campaigns.

## Focus Areas
- Exploratory Data Analysis

- Retention rate analysis

- Data visualization and business analysis

- Time-based Cohort analysis with Python

## Business Overview/Problem
### Business Problem

E-Shop Pro is facing a significant challenge in retaining its customers. While the company has been successful in acquiring new customers through marketing efforts and promotions, it has noticed a concerning trend of declining customer retention rates. This is a critical issue in the highly competitive e-commerce industry, where customer loyalty and repeat business are paramount.

While E-Shop Pro collects vast amounts of customer data, it has not effectively leveraged this data to gain insights into customer behavior and preferences. This underutilization of data is a missed opportunity for improving customer retention.

## Rationale for the Project
- Cohort analysis is a powerful technique used in business analytics to gain insights into the behavior and characteristics of specific groups of customers, users, or any defined segments over time. These groups, or cohorts, share a common characteristic or experience, such as the date of their first purchase, sign-up, or acquisition.

- The primary purpose of cohort analysis is to track how different cohorts perform and behave over their entire lifecycle, allowing businesses to understand their customer retention, engagement, and revenue generation patterns. Here's a breakdown of key elements in cohort analysis:

 - Cohort analysis is a valuable tool in the realm of customer retention because it provides businesses with a deeper understanding of customer behavior over time. Here are reasons why carrying out cohort analysis is essential for assessing and improving customer retention:

### A. Identifying Retention Trends: 
cohort analysis allows businesses to group customers based on specific criteria, such as their acquisition date or first purchase. By tracking these cohorts over time, it becomes easier to identify trends in customer behavior, including whether retention rates are increasing or decreasing.
 
### B. Pinpointing Churn Patterns:
Cohort analysis helps in identifying when and why customers tend to churn or stop making repeat purchases. This information is crucial for devising strategies to reduce churn and retain more customers.

## Aim of the Project
This project aims to gain comprehensive insights into user engagement patterns and content preferences on the Streamlix platform. By conducting a rigorous cohort analysis, Streamlix seeks to understand the evolving behavior of distinct user groups over specific timeframes. This endeavor is driven by the imperative to refine content curation, optimize user experience, and ultimately, maximize user satisfaction.

## Data Description

- InvoiceNo: A unique identifier for each invoice or transaction, often used for tracking and reference purposes.

- StockCode: A code or identifier associated with a specific product or item in the e-commerce store's inventory, used for cataloging and tracking purposes.

- Description: A categorical feature that provides a brief textual description of the product or item being sold, offering clarity to customers about what they are purchasing.

- Quantity: The quantity or number of units of a product that were included in the transaction, indicating the purchase volume for each item.

- InvoiceDate: The date and time when the transaction or invoice was generated, offering insights into when purchases were made and allowing for temporal analysis.

- UnitPrice: Indicating the total cost of the items purchased.

- CustomerID: A unique identifier associated with each customer or shopper, allowing for customer-specific analysis and tracking of individual purchasing behavior.

- Country: The name of the country where the customer is located or where the transaction occurred.

## Tech Stack
- Programming language – Python

## Libraries
- Numpy: For performing mathematical operations over data
- Pandas: For Data Analysis and Manipulation
- Matplotlib.pyplot: For Data Visualization
- Seaborn: For Data Visualization
- Scikit-learn: For Machine Learning
## Code Section
## Import Libraries
```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
import datetime as dt
from matplotlib.ticker import FuncFormatter
```

## Load and prepare dataset
```python
# Load daatset

data = pd.read_csv("ecommerce_cohort_analysis.csv")
data
```
```python
data.describe(include ="all")
```
```python
data.info()
```
```python
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 541909 entries, 0 to 541908
Data columns (total 8 columns):
 #   Column       Non-Null Count   Dtype  
---  ------       --------------   -----  
 0   InvoiceNo    541909 non-null  object 
 1   InvoiceDate  541909 non-null  object 
 2   CustomerID   406829 non-null  float64
 3   StockCode    541909 non-null  object 
 4   Description  541909 non-null  object 
 5   Quantity     541909 non-null  int64  
 6   UnitPrice    541909 non-null  float64
 7   Country      541909 non-null  object 
dtypes: float64(2), int64(1), object(5)
memory usage: 33.1+ MB
```
```python
data.isnull().sum()
```

```python
InvoiceNo           0
InvoiceDate         0
CustomerID     135080
StockCode           0
Description         0
Quantity            0
UnitPrice           0
Country             0
dtype: int64
```
```python
# Drop missing columm
data.dropna(inplace = True)

#we can only track the activities of customers that are known (those with customerID)
```
```python
data["InvoiceDate"] = pd.to_datetime(data["InvoiceDate"])
```

## Exploratory Data Analysis

### Analysis By Country

### Bivariate Analysis of countries and total number of purchase 
```python
quantity_per_country = data.groupby(["Country"])["Quantity"].sum().reset_index()
quantity_per_country = quantity_per_country.sort_values("Quantity", ascending = False).reset_index()
```
```python
quantity_per_country.head(3)
```
```python
# Draw a plot of top 10 countries with highest number of quantity purchase
top_10_countries = quantity_per_country.head(10)

# Create a bar chart from the selected data
plt.figure(figsize=(20, 5))

y_format = FuncFormatter(lambda x, _: f'{x / 1000:.1f}K')
ax = sns.barplot(x='Country', y='Quantity', data= top_10_countries)
ax.yaxis.set_major_formatter(y_format)

plt.xlabel('Countries')
plt.ylabel('Total quantity purchased in (1000s)')
plt.title('Bar Chart of top 10 countries with highest purchase')
plt.show()
```

![image](https://github.com/user-attachments/assets/c34d92c0-bcf2-4128-ba7a-6c15d89d6012)

```python
# Draw a plot of 10 countries with least number of quantity purchase
least_10_countries = quantity_per_country.tail(10)

# Create a bar chart from the selected data
plt.figure(figsize=(20, 5))

y_format = FuncFormatter(lambda x, _: f'{x / 1000:.1f}K')
ax = sns.barplot(x='Country', y='Quantity', data= least_10_countries)
ax.yaxis.set_major_formatter(y_format)

plt.xlabel('Countries')
plt.ylabel('Total quantity purchased in (1000s)')
plt.title('Bar Chart of countries with least purchase')
plt.show()
```
![image](https://github.com/user-attachments/assets/640f9bf8-a004-47db-9452-0e478c121dc7)

## Bivariate Analysis of countries and number of customers

```python
country_to_customers = data.groupby(["Country"])["CustomerID"].nunique().reset_index()
country_to_customers = country_to_customers.sort_values("CustomerID", ascending = False).reset_index()
country_to_customers.rename(columns = {"CustomerID" : "Number of customers"}, inplace = True)
```
```python
country_to_customers.head(3)
```
```python
# Draw a plot of top 10 countries with highest number of customers (unique customerID)
top_countries_to_customers = country_to_customers.head(10)

# Create a bar chart from the selected data
plt.figure(figsize=(20, 5))

sns.barplot(x='Country', y='Number of customers', data= top_countries_to_customers)

plt.xlabel('Countries')
plt.ylabel('Total number of customers')
plt.title('Bar Chart of top 10 countries with highest number of customers')
plt.show()
```
![image](https://github.com/user-attachments/assets/4e22b734-99d8-44c5-8969-ade267f3b0a8)

## Sales Trend 
```python
# Group the data by month and calculate the total monthly sales
monthly_sales = data.groupby(data['InvoiceDate'].dt.to_period('M'))['Quantity'].sum()
```
```python
# Create a line chart
plt.figure(figsize=(20, 10))
plt.plot(monthly_sales.index.strftime('%Y-%m'), monthly_sales.values, marker='o', linestyle='-')
plt.xlabel('Month')
plt.ylabel('Total Sales')
plt.title('Monthly Sales Trend')
plt.grid(True)

# Display the plot
plt.show()
```
![image](https://github.com/user-attachments/assets/78370b28-2554-4b3c-9ed6-787b5923ef49)
## Observations 
- December 2010 to August 2011: noticeable fluctuations in sales quantity.
- August 2011 to November 2011: A significant sales increase occurred.
- November 2011 to December 2011: Sales experienced a sudden and substantial decline.

## Cohort Analysis 
### Create cohort date
Since we are carrying out a time-based cohort analysis, the cohorts will be grouped according to the dates they made their first purchase and retention rate will be calculated by analyzing the months that they stayed active after their first purchase.

```python
#create Invoice month as new feature
def get_month(x):
    return dt.datetime(x.year, x.month, 1)
'''
```python
data["InvoiceDate"]  = data["InvoiceDate"].apply(get_month)
data.tail()
```
```python
def get_cohort_date(data):
    """ this function takes in the dataframe
        and returns the cohort date

        variables:
        data = dataframe
        cohort date = the first date they made a purchase
        """

    # assign the minimum date to all unique candidateID, i.e the first day they made a purchase
    data["cohort date"] = data.groupby("CustomerID")["InvoiceDate"].transform("min")

    return data["cohort date"]
```
```python
#apply the function created to our dataframe and extract the 2 newly created column

data["cohort date"] = get_cohort_date(data)
```
```python
data
```
## Create cohort index 
A cohort index is a numerical representation that measures the time interval in months since a particular group (cohort) made their initial purchase. For example an index of 4 indicates that this cohort made their first purchase four months ago. 
This will be calculated by substracting the time the customer made their first purchase (cohort date) from the recent purchase time (invoice date)
```python
#create a function that extracts the year and month from the first and last cohort date

def get_year_and_month(data, col):
    """
    This function takes in the dataframe and column,
    and returns the month and year component for that column

    Variables:
    data = dataframe
    col = column
    month = month component
    year = year component"""

    month = data[col].dt.month
    year = data[col].dt.year
    return month, year
```
```python
# apply the fucntion on cohort first date column
first_month, first_year = get_year_and_month(data,"cohort date")
```
```python
first_month
```
```python
0         12
1         12
2         12
3         12
4         12
          ..
541904     8
541905     8
541906     8
541907     8
541908     8
Name: cohort date, Length: 406829, dtype: int64
```
```python
# apply the fucntion on cohort latest date column
latest_month, latest_year = get_year_and_month(data,"InvoiceDate")
```
```python
latest_month
```
```python
0         12
1         12
2         12
3         12
4         12
          ..
541904    12
541905    12
541906    12
541907    12
541908    12
Name: InvoiceDate, Length: 406829, dtype: int64
```
## Create Cohort Index
```python
# write a function to create cohort index
def create_cohort_index(first_month, first_year, latest_month,latest_year):
    """
    This code creates takes in the first and latest month and year
    and returns the calculated period(in months) the customer has been active

    variables:
    first_month: first month the customer made purchase
    first_year: first year the customer made purchase

    latest_month: recent month the customer made purchase
    latest_year: recent year the customer made purchase

    index: The duration between first and latest purchase (in months)"""

    year_diff = latest_year - first_year
    month_diff = latest_month - first_month
    index = year_diff*12 + month_diff +1 # +1 is added because of customers who have been active for just 1 month
    return index
```
```python
data["cohort_index"] = create_cohort_index(first_month, first_year, latest_month, latest_year)
```
```python
data
```
## Create a Pivot table
To create a pivot table first we need to know how many customers made a purchase each month after their first purchase 
for example how many customers from the September cohort made a purchase four months after their initial purchase in September? 
## Cohort table 
```python
cohort_info = data.groupby(["cohort date","cohort_index"])["CustomerID"].nunique().reset_index()
```
```python
cohort_info.rename(columns = {"CustomerID": "Number of customers"}, inplace = True)
```
```python
cohort_info
```
## Pivot table
```python
# create a pivot table

cohort_table = cohort_info.pivot(index = "cohort date", columns = ["cohort_index"], values = "Number of customers")

#change index to understandable format
cohort_table.index = cohort_table.index.strftime('%B %Y')
cohort_table
```
```python
#visualize our results in heatmap

plt.figure(figsize = (21,10))
sns.heatmap(cohort_table, annot = True, cmap = 'Dark2_r')
```
![image](https://github.com/user-attachments/assets/52585168-5a53-4ddc-b0b6-31eb388ca981)
## Observe retention rate
To efficiently observe the customer retention rate, let's plot the chart in percentage 

```python
# showing retention rate in percentage
new_cohort_table = cohort_table.divide(cohort_table.iloc[:,0], axis = 0)
new_cohort_table
```
## Draw heatmap
```python
#visualize our results in heatmap

plt.figure(figsize = (21,10))
sns.heatmap(new_cohort_table, annot = True, cmap = 'Dark2_r', fmt = '.0%')
```
![image](https://github.com/user-attachments/assets/69bb9ec7-e8a7-4684-afa4-4748cd198c31)
The heatmap above illustrates the customer retention rates for each cohort.
## Observations
A healthy retention rate for e-commerce platforms is typically considered to be in the range of 20% to 40%. This means that 20% to 40% of your customers continue to make purchases from your e-commerce platform after their initial purchase.
- December 2010 Cohort Outperforms Others: The fact that the December 2010 cohort has a retention rate above 30% is a positive sign. It suggests that this group of customers has remained engaged with your e-commerce platform over time. This could be due to various factors, such as the quality of your products/services, effective marketing, or a strong customer retention strategy.
- Decline in December 2011: The observation that all cohorts have low retention rates in December 2011 suggests that there may have been specific challenges or issues affecting customer retention during that time. It's important to investigate what might have caused this decline and whether it's a one-time event or a recurring pattern
Variability in Retention Rates: The range of retention rates, from a minimum of 8% to a maximum of 50%, suggests that there is significant variability in how different cohorts of customers are behaving. While 8% is relatively low, 50% is relatively high, considering the standard e-commerce retention rates mentioned earlier.

## Recommendations
Identify Factors Driving High Retention (December 2010): Analyze what factors have contributed to the high retention rate for the December 2010 cohort. Was there a specific marketing campaign, product improvement, or customer engagement strategy that worked well for this group? Try to replicate successful strategies for other cohorts.
- Investigate December 2011 Drop: Investigate why all cohorts have low retention rates on December 2011. It might involve analyzing customer feedback, product quality, customer service, or any changes in your business operations during that time. Identifying and addressing the root causes of this drop is crucial for improving future retention rates.
- Set Realistic Targets: While the standard e-commerce retention rate range is 20% to 40%, it's essential to set targets that are specific to your business and its circumstances. Aim to improve retention rates gradually over time based on your historical data and industry benchmarks.
- Implement Retention Strategies: Develop and implement retention strategies that are tailored to different cohorts of customers. Personalized marketing, loyalty programs, and targeted communication can help improve retention rates.

In addition t observing the behavior of each cohort, we can create a table which shows the average quantity of product bought by each cohort and how it fluctuates.

## Average quantity bought 
```python
average_quantity = data.groupby(["cohort date", "cohort_index"])["Quantity"].mean().reset_index()
average_quantity["Quantity"] = average_quantity["Quantity"].round(1)
```
```python
average_quantity.rename(columns = {"Quantity": "average quantity"}, inplace = True)
```
```python
average_quantity
```
```python
# create a pivot table

quantity_table = average_quantity.pivot(index = "cohort date", columns = ["cohort_index"], values = "average quantity")

#change index to understandable format
quantity_table.index = quantity_table.index.strftime('%B %Y')
quantity_table
```
```python
#visualize our results in heatmap

plt.figure(figsize = (21,10))
sns.heatmap(quantity_table, annot = True, cmap = 'Accent_r')
```
![image](https://github.com/user-attachments/assets/0bcff8da-72b0-487c-9458-53d111fa15b9)
## Observations 
Although there is significant drop in customer retention after their first month as observed on the previous chart, the average quantity bought is not experiencing much fluctuations. Meaning that there is possibility that a few customers tend to purchase a lot of product, therefore maintaining the average value. 
## Recommendation
This can serve as a signal to carryout targetted marketing in countries where more quantities are sold in contrast to targetting countries where more customers are acquired. 


