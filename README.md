#Credit Card Analysis
This repository contains a detailed analysis of credit card customer and transaction data. The goal is to identify key trends, segment customers, and derive actionable insights to inform business strategy. The final analysis is presented in a Power BI dashboard.

Table of Contents
Problem Statement

Dataset Description

Data Cleaning and Preparation

Data Analysis

Problem Statement
The primary objective of this project is to analyze a credit card dataset to understand customer behavior and identify the key factors driving revenue. The analysis aims to answer the following questions:

Who are the most profitable customer segments based on demographics like job, income, and education?

Which credit card products are the most popular and generate the most revenue?

What are the primary spending categories for customers?

How do transaction patterns and revenue vary over time (e.g., quarterly)?

What are the key drivers of interest income?

The insights from this analysis will help in creating targeted marketing campaigns, improving customer retention, and making strategic decisions for the business.

Dataset Description
The analysis utilizes two separate datasets, which are linked by a common client number.

Customer Data (customer.csv): This file contains demographic information about each credit card holder.

Key Columns:

Client_Num: Unique identifier for each customer.

Customer_Age: Age of the customer.

Age_Group: Categorical grouping of customer ages (e.g., 20-30, 31-40).

Gender: Gender of the customer (M/F).

Dependent_Count: Number of dependents for the customer.

Education_Level: Highest level of education attained.

Marital_Status: Marital status of the customer.

state_cd: The state where the customer resides.

Customer_Job: The profession of the customer.

Income: Annual income of the customer.

Income_Group: Categorical grouping of income levels (e.g., Low, Medium, High).

Car_Owner: Indicates if the customer owns a car (yes/no).

House_Owner: Indicates if the customer owns a house (yes/no).

Personal_Loan: Indicates if the customer has a personal loan (yes/no).

Contact: Method of contact (e.g., cellular, unknown).

Cust_Satisfaction_Score: A score from 1-5 indicating customer satisfaction.

Credit Card Transaction Data (credit_card.csv): This file contains detailed information about transactions and card usage.

Key Columns:

Client_Num: Unique identifier for each customer (acts as a foreign key).

Card_Category: Type of credit card (e.g., Blue, Silver, Gold, Platinum).

Annual_Fees: The annual fee for the credit card.

Week_Start_Date: The start date of the week for the transaction.

Credit_Limit: The credit limit on the card.

Total_Trans_Amt: The total amount of transactions.

Total_Trans_Vol: The total volume (count) of transactions.

Avg_Utilization_Ratio: The ratio of credit used to the credit limit.

Exp Type: The category of the expense (e.g., Bills, Travel, Food).

Interest_Earned: The amount of interest earned from the customer.

Data Cleaning and Preparation
Before analysis, the data was imported and transformed using Power Query in Power BI.

Steps to Import Data as a Folder
Navigate to Get data -> More... -> All -> Folder and click Connect.

Provide the path leading to the folder containing the datasets and click OK.

Click on Transform Data to open the Power Query editor.

Calculated Fields (DAX)
After loading the data into the model, several calculated fields were created using DAX to enhance the analysis:

Income Group: A calculated column to segment customers by income brackets.

Income Group = 
SWITCH(
    TRUE(),
    'customer'[Income] < 35000, "Low",
    'customer'[Income] >= 35000 && 'customer'[Income] < 70000, "Med",
    'customer'[Income] >= 70000, "High",
    "unknown"
)

Age Group: A calculated column to segment customers by age.

Age_Group = 
SWITCH(
    TRUE(),
    'customer'[Customer_Age] < 30, "20-30",
    'customer'[Customer_Age] >= 30 && 'customer'[Customer_Age] < 40, "30-40",
    'customer'[Customer_Age] >= 40 && 'customer'[Customer_Age] < 50, "40-50",
    'customer'[Customer_Age] >= 50 && 'customer'[Customer_Age] < 60, "50-60",
    'customer'[Customer_Age] >= 60, "60+",
    "unknown"
)

Week Num2: A calculated column to get the week number from the start date.

Week Num2 = WEEKNUM(credit_card[Week_Start_Date])

Revenue: A calculated column to get the total revenue per transaction record, including fees.

Revenue = 'credit_card'[Annual_Fees] + 'credit_card'[Total_Trans_Amt] + 'credit_card'[Interest_Earned]

Current Week Revenue: A measure to calculate the revenue for the most recent week in the dataset.

Current_Week_Revenue = 
CALCULATE(
    SUM('credit_card'[Revenue]),
    FILTER(
        ALL(credit_card),
        'credit_card'[Week_Num2] = MAX('credit_card'[Week_Num2])
    )
)

Previous Week Revenue: A measure to calculate the revenue for the week prior to the most recent week.

Previous_Week_Revenue = 
CALCULATE(
    SUM('credit_card'[Revenue]),
    FILTER(
        ALL(credit_card),
        'credit_card'[Week_Num2] = MAX('credit_card'[Week_Num2]) - 1
    )
)

Data Analysis
The cleaned and merged dataset was used to build an interactive dashboard in Power BI.

Key Insights
Insights from Customer Dashboard
Income and Occupation Drive Revenue: Businessmen are the most significant contributors to both income and revenue, generating $17.4 million in revenue from $187 million in income. White-collar professionals follow, contributing $10.1 million in revenue. This suggests that high-earning professionals are the most lucrative customer segment.

Education Level Correlates with Revenue: Graduates are the highest revenue-generating education segment, contributing $12 million, followed by post-graduates at $10 million.

Marital Status Insights: Married customers generate slightly more revenue ($15 million) than single customers ($12 million).

Geographic Concentration: The top three revenue-generating states are Texas (TX), New York (NY), and California (CA), each contributing between $6 million and $7 million in revenue.

Gender-Based Revenue Fluctuation: The "Revenue vs Gender" chart shows that revenue from both genders fluctuates throughout the year. Notably, one gender (represented by the blue line) shows higher peaks, reaching approximately $0.72 million around April 2023, while the other (yellow line) remains relatively lower.

Insights from Transaction Dashboard
Primary Revenue Drivers by Card Type: The "Blue" card is the dominant product, accounting for $46.1 million (approximately 83%) of the total revenue. In contrast, premium cards like "Platinum" contribute only $1.1 million.

Top Spending Categories: The largest expense type is "Bills," which accounts for $14 million in revenue. This indicates that many customers use their credit cards for routine, high-value payments.

Transaction Method Dominance: "Swipe" transactions are the most common method of use, driving $35 million in revenue, significantly more than Chip or Online methods.

Quarterly Performance: Revenue and transaction counts remain relatively stable across all four quarters. Q3 saw the highest revenue at $14.2 million with 166.6K transactions, while Q2 had the highest transaction volume at 168.5K for $14.0 million in revenue.

Actionable Strategic Insights
Target High-Value Segments: Focus marketing and retention efforts on "Businessmen," "White-collar" professionals, and "Graduates," as they are the most profitable segments. Tailor premium benefits to attract and retain these high-income customers.

Promote Bill Payments: Since "Bills" are the top spending category, consider launching campaigns that offer minor rewards or cashback for utility, rent, or subscription payments to encourage even greater usage.

Enhance the "Blue Card" Proposition: Given the "Blue" card's overwhelming popularity, there is an opportunity to introduce tiered benefits within this category to increase engagement and revenue without requiring customers to switch to a different card type.

Investigate Usage Methods: The dominance of "Swipe" transactions is unusual in a market moving towards Chip and Online payments. It would be beneficial to analyze if this is due to specific merchant categories or a lack of customer adoption of newer technologies.

KPIs and Metrics
Calculated key metrics such as Total Revenue ($55.3M), Total Transaction Amount ($44.5M), and Total Interest Earned ($7.8M) to provide a high-level summary of business performance.
