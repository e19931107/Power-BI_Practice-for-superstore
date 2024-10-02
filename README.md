# Power-BI_RFM-analysis-for-superstore-dataset

## Data source:
https://www.kaggle.com/datasets/bravehart101/sample-supermarket-dataset

## Introduction
Super store data is popular in Kaggle.
There are 3 dimensions that we can analyze: 
1. Timeline: When customer purchase the products, When the products shipped out.
2. Category: Category and sub-category
3. Location: Where are the customers?

Therefore, there are 1 sheet for summary analysis and 3 sheets analyzed by timeline, category and location, to make deeper analysis.

## Data ETL
The dataset is really clear and organized; therefore, I didn't speend too much effort on data ETL.
Only Date Format needs to be adjusted.
### Before
![image](https://github.com/user-attachments/assets/2d266e4c-e3a1-4dad-a7fd-b405214e6fc7)

### After
![image](https://github.com/user-attachments/assets/6d8a50c7-7616-4089-bb2c-2f1299905ce7)

## 2 kind of RFM analysis
I found there are two type of RFM analysis, 1st one is grouping customers into 8 segments; 2nd one is grouping customers into 11 segments.

### 8 segments:

https://powerbiacademy.medium.com/power-bi-rfm-%E6%A8%A1%E5%9E%8B-%E6%95%99%E4%BD%A0%E5%A6%82%E4%BD%95%E7%B4%B0%E7%B7%BB%E5%8C%96%E7%B6%93%E7%87%9F-crm-%E5%AE%A2%E6%88%B6%E9%97%9C%E4%BF%82-a09fc823a811

### 11 segments:

https://gustiyaniz.medium.com/unleashing-customer-insights-a-guide-to-rfm-analysis-using-power-bi-648a1f56f036

## Step by Step RFM analysis
### Step 1:
In train dataset, I added two columns: Last Date to Purchase and R(Recency)

> Last Date to Purchase = MAXX(
    FILTER('train',EARLIER('train'[Customer ID])='train'[Customer ID]),'train'[Order Date].[Date])

> R = DATE(2019,1,1)-'train'[Last Date to Purchase].[Date]

I set 2029/01/01 becasue later on when we present the number on the dashboard, the number won't be too large.

### Step 2:
I generated new table from original dataset, this table I called it "RFM"
In this RFM table, it will identify customers' Recency, Frequency and Monetary.
> RFM = SUMMARIZE('train','train'[Customer ID],train[R],"F",'train'[F],"M",'train'[M])

![image](https://github.com/user-attachments/assets/7c6fea01-a4b0-4549-98d5-3d2174264672)

### Step 3:
Rating for each customers' Recency, Frequency and Monetary

> R_rate = IF('RFM'[R]<=AVERAGE(RFM[R]),1,2)

> F_rate = IF('RFM'[F]>=AVERAGE(RFM[F]),2,1)

> M_rate = IF('RFM'[M]>=AVERAGE(RFM[M]),2,1)



## Result
### Summary
![image](https://github.com/user-attachments/assets/499e860f-f536-483c-b351-7565d0c2f54f)

### By Year
![image](https://github.com/user-attachments/assets/5b5702be-1eda-48eb-b5c0-7d1e3ff29e8d)

### By Category
![image](https://github.com/user-attachments/assets/14582621-b203-48e5-8985-1e4f4384da08)

### By State
![image](https://github.com/user-attachments/assets/114ddef5-1e65-426f-9c0a-31ca5b59dfd2)

### RFM analysis
![image](https://github.com/user-attachments/assets/92fc020d-b173-46ad-bd88-870b6d62a81a)

