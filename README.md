# Projcet Title: RFM Analysis Using SQL on the AdventureWorks Dataset (2011-2014)

## Table of Contents
- [Overview](#overview)
- [Dataset](#dataset)
- [Objective](#objective)
- [Analysis Approach](#analysis-approach)
- [Association Rule Metrics](#Association-Rule-Metrics)
- [Key Findings](#key-findings)
- [How to Use](#how-to-use)
- [Technologies Used](#technologies-used)
- [Results](#results)
- [Recommendation](#recommendation)
- [Contact](#contact)

## Overview:

This project aims to perform RFM (Recency, Frequency, Monetary) analysis on the AdventureWorks2022 dataset, using SQL to segment customers based on their purchasing behavior. By evaluating recency, frequency, and monetary value of customer interactions, the project seeks to uncover actionable insights for customer segmentation and retention strategies. The RFM model allows businesses to identify valuable customers, develop targeted marketing strategies, and enhance customer engagement.

## Dataset

The analysis is based on the AdventureWorks2022, obtained from Microsoft Learn:

🔗 AdventureWorks sample Databases
- Source: [Microsoft Learn](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2022.bak)
- Time Period Covered: 2011-2014

This dataset contains transactional data from AdventureWorks, a fictional retail company, including sales, products, and customer orders.

## Objective

This project aims to perform RFM analysis using SQL on the AdventureWorks dataset to segment customers based on their purchasing behavior. By analyzing recency, frequency, and monetary value, it identifies key customer groups such as VIPs, Loyal Customers, and At-Risk Customers. The insights gained will help optimize retention strategies, enhance customer engagement, and improve marketing efforts. Using SQL for automation ensures a scalable and efficient approach to customer segmentation and decision-making.

## Analysis Approach
1. Customer Segmentation:
- Recency (R): Measures how recently a customer made a purchase. Customers who have made recent purchases are more likely to buy again.
- Frequency (F): Tracks how often a customer makes a purchase. Frequent buyers tend to show more loyalty and engagement.
- Monetary (M): Assesses the amount of money a customer spends. High spenders contribute significantly to the business's profitability.
2. RFM Scoring:
- The customers are scored for each of the three dimensions (Recency, Frequency, Monetary), with scores from 1 to 5, where 5 represents the best behavior.
3. Segmenting Customers:
- Customers are divided into distinct groups based on their RFM scores (e.g., VIP Customers, Loyal Customers, At-Risk Customers) like in [RFM Segment](RFM_Segments.xlsx)
4. Interpret Results:
- Analyze segment behavior to uncover patterns and opportunities.
5. Recommendations:
- Develop strategies tailored to each segment based on their specific behaviors and potential value.

## Key Findings

In this RFM project, we will identify which customers fall into key segments such as VIP Customers, Loyal Customers, and others segments based on their Recency, Frequency, and Monetary scores like in [RFM Segment](RFM_Segments.xlsx). This analysis will help pinpoint the most valuable customers, as well as those who may need targeted engagement to boost loyalty and spending.

## How to use
1. Restore database in SSMS as guided in Mirosoft Learn [Restore to SQL Server](https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms)
2. Using SQL Server Management Studio (SSMS) to execute SQL queries

## Technologies Used
- SQL code: SQL queries were used to preprocess data, calculate Recency, Frequency, and Monetary scores, segment customers, and identify patterns in customer behavior, enabling effective customer segmentation and targeted marketing strategies.

## Results 
- The results are stored in the file [RFM_result.csv](RFM_result.csv)

![RFM_result](https://github.com/user-attachments/assets/2d1069d8-e131-4605-9502-eed87e979bef)

Figure 1: Sample RFM Results

## Recommendation

1. VIP Customers (R = 5, F = 5, M = 5)
  Offer exclusive perks, personalized engagement, premium support, and referral incentives.
2. Loyal Customers (R = 4-5, F = 4-5, M = 4-5)
  Enhance loyalty programs, upsell, provide exclusive offers, and maintain consistent communication.
3. Potential Loyalists (R = 4, F = 1-3, M = 1-3)
  Encourage repeat purchases, educate with content, offer time-sensitive promotions, and personalized outreach.
4. Big Spenders (R = 5, F = 4-5, M = 4-5)
  Offer premium product discounts, exclusive perks, event invitations, and FOMO-driven deals.
5. Promising Customers (R = 3-4, F = 4, M = 1-3)
  Increase engagement with targeted campaigns, bundle deals, and promotions to boost spending.
6. At Risk Customers (R = 3, F = 1-3, M = 1-3)
  Use win-back campaigns, personalized incentives, surveys, and retargeting ads.
7. Churned Customers (R = 1-2, F = 1-2, M = 1-2)
  Reactivate with strong incentives, feedback loops, re-engagement emails, and personalized recommendations.
8. Other Segments (Mixed R, F, M)
  Provide targeted promotions, general engagement, and track behavior to move customers to higher-value segments.

## Contact

📧 Email: pearriperri@gmail.com

🔗 [LinkedIn](https://www.linkedin.com/in/phan-chenh-6a7ba127a/) | Portfolio
