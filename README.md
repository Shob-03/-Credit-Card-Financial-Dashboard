# 💳 Credit Card Financial Dashboard — SQL + Power BI
End-to-end credit card financial dashboard built with SQL Server and Power BI — analyzing revenue, transactions, customer demographics and card performance

## 📌 Objective
Designed and developed an end-to-end credit card financial dashboard 
to analyze weekly transaction trends, revenue performance, customer 
demographics, and card category insights across 1,000 customers.

## 📂 Dataset
- Source: Synthetically generated dataset inspired by real 
  credit card customer data from Kaggle
- Records: 1,000 customers | 1,000 weekly transaction records
- Period: January 2024 – December 2025
- Tools Used: MySQL, Power BI, DAX

## 🗄️ Database Setup
Data was loaded into MySQL Server using the following steps:
- Created `creditcard_database` with two tables:
  - `creditcard_detail` — weekly transaction records
  - `customer_detail` — customer demographic information
- Data imported using `LOAD DATA INFILE` command
- Tables joined on `Client_Num` as the primary key

## 📊 Dashboard Pages

### Page 1 — Credit Card Transaction Report
- Total Revenue: ₹5.49M | Total Transactions: 65K
- Total Interest Earned: ₹765.45K
- Revenue breakdown by Quarter, Age Group, Expenditure Type,
  Education, Customer Job, Card Type and Chip Type

### Page 2 — Credit Card Customer Report
- Total Income: ₹57M | Avg Customer Age: 48
- Weekly revenue trend (Jan 2024 – Dec 2025)
- Customer breakdown by Education Level, Satisfaction Score,
  Car Ownership and Income Distribution by Job

## 🔍 Key Insights
- **Blue card holders generate the most revenue** at ₹4.99M — 
  significantly higher than Silver, Gold and Platinum combined
- **Online transactions lead** chip type revenue at ₹1.98M, 
  followed by Swipe (₹1.9M) and Chip (₹1.6M)
- **Customers aged 60+ generate the highest revenue** at ₹1.46M, 
  making seniors the most valuable segment
- **Businessmen and self-employed customers** contribute the most 
  revenue among all job categories
- **Q1 recorded the highest quarterly revenue** at ₹1.50M
- **51.6% of customers do not own a car** suggesting a predominantly 
  urban customer base

## 🧮 DAX Measures & Calculated Columns

### Calculated Columns
```dax
-- Age Group segmentation
Age_Group = 
SWITCH(TRUE(),
    'customer_detail'[Customer_Age] < 30, "20-30",
    'customer_detail'[Customer_Age] >= 30 && 'customer_detail'[Customer_Age] < 40, "30-40",
    'customer_detail'[Customer_Age] >= 40 && 'customer_detail'[Customer_Age] < 50, "40-50",
    'customer_detail'[Customer_Age] >= 50 && 'customer_detail'[Customer_Age] < 60, "50-60",
    'customer_detail'[Customer_Age] >= 60, "60+",
    "Unknown"
)

-- Income Group segmentation
Income_Group = 
SWITCH(TRUE(),
    'customer_detail'[Income] < 50000, "Low",
    'customer_detail'[Income] >= 50000 && 'customer_detail'[Income] < 100000, "Medium",
    'customer_detail'[Income] >= 100000 && 'customer_detail'[Income] < 150000, "High",
    "Very High"
)
```

### Measures
```dax
-- Week number calculation (Monday as start of week)
Week_Number = WEEKNUM('creditcard_detail'[Week_Start_Date], 2)

-- Current week revenue
Current_Week_Revenue = 
CALCULATE(
    SUM(creditcard_detail[Revenue]),
    FILTER(ALL(creditcard_detail),
    'creditcard_detail'[Week_Number] = MAX('creditcard_detail'[Week_Number]))
)

-- Previous week revenue
Previous_Week_Revenue = 
CALCULATE(
    SUM(creditcard_detail[Revenue]),
    FILTER(ALL(creditcard_detail),
    'creditcard_detail'[Week_Number] = MAX('creditcard_detail'[Week_Number]) - 1)
)

-- Week over week revenue growth
WoW_Revenue_Growth = 
DIVIDE(
    ([Current_Week_Revenue] - [Previous_Week_Revenue]),
    [Previous_Week_Revenue]
)
```
## 🛠️ Tools
- MySQL — database creation and data loading
- Power BI — dashboard and visualizations
- DAX — calculated measures and columns
