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


----------------

## Project Overview: 

Dataset: AdventureWorks2022.bak (https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms)

RFM analysis is a basic model for customer segmentation or customer segmentation based on customer buying behavior. By doing this, you gain insights into which customers are the most valuable to your business, how recently they interacted with your business, how often they make purchases, and how much they spend.

## Objectives:

This project aims to perform RFM analysis using SQL on the AdventureWorks dataset to segment customers based on their purchasing behavior. By analyzing recency, frequency, and monetary value, it identifies key customer groups such as VIPs, Loyal Customers, and At-Risk Customers. The insights gained will help optimize retention strategies, enhance customer engagement, and improve marketing efforts. Using SQL for automation ensures a scalable and efficient approach to customer segmentation and decision-making.

## Project Structure:
1. Identify Customer Segments:
- Recency (R): How recently did the customer make a purchase? Customers who 
bought recently are more likely to buy again.
- Frequency (F): How often does the customer make a purchase? Frequent buyers are often more loyal and engaged.
- Monetary (M): How much money does the customer spend? High spenders are more profitable.

2. Focus on the Right Marketing Tactics:
By analyzing each segment’s behavior, you can direct marketing efforts where 
they will be most effective.

3. To use bak file in mssql:
- Save the file in C disk
- open tthe file in mssql: go to database -> restore database -> device -> three dots (...)  -> add -> C disk -> find the file -> Ok -> Ok -> done 

---

## Developing Strategies for Each Segment:
1. VIP Customers (R = 5, F = 5, M = 5)
- Description: Most valuable customers who are recent, frequent, and high spenders.

Strategy:
- Reward and Retain: Offer exclusive perks (e.g., VIP loyalty programs, early access to new products or sales).
- Personalized Engagement: Send personalized offers, thank-you messages, and invites to special events.
- Premium Support: Provide premium customer service (e.g., dedicated account managers).
- Referrals: Encourage them to refer friends or family for exclusive benefits.

2. Loyal Customers (R = 4-5, F = 4-5, M = 4-5)
- Description: High frequency of purchase and moderate-to-high monetary value.

Strategy:
- Loyalty Program: Introduce or enhance a rewards program to encourage continued spending.
- Upsell and Cross-sell: Recommend complementary products based on their purchase history.
- Exclusive Offers: Provide loyalty discounts or early access to sales.
- Consistent Communication: Keep them engaged with regular updates on new products, services, or events.

3. Potential Loyalists (R = 4, F = 1-3, M = 1-3)
- Description: Recent customers with moderate frequency and monetary value.

Strategy:
- Encourage Repeat Purchases: Send follow-up emails with discounts or promotions to entice them to buy again.
- Educate and Engage: Introduce them to a wider range of products through educational content or product recommendations.
- Personalized Outreach: Reach out via targeted messaging to make them feel valued and encourage loyalty.
- Time-Limited Offers: Give them special time-sensitive deals to encourage quicker repeat purchases.

4. Big Spenders (R = 5, F = 4-5, M = 4-5)
- Description: High spenders who may not purchase frequently.

Strategy:
- High-Value Offers: Provide offers on premium products or exclusive discounts for large purchases.
- Focus on Exclusivity: Offer VIP-style perks such as free shipping, early product releases, or personalized services.
- Event Invitations: Invite them to special events or experiences to maintain engagement.
- Create FOMO: Use time-sensitive offers to encourage higher purchase frequency.

5. Promising Customers (R = 3-4, F = 4, M = 1-3)
- Description: Recent but with low frequency and monetary value.

Strategy:
- Increase Engagement: Encourage more frequent purchases through targeted campaigns or rewards programs.
- Incentivize Higher Spending: Offer discounts or promotions to boost their spend per transaction.
- Regular Communication: Keep them engaged with new product updates or content that adds value.
- Introduce Bundle Deals: Provide value through bundle offers or "buy more, save more" promotions.

6. At Risk Customers (R = 3, F = 1-3, M = 1-3)
- Description: Previously frequent but less recent customers.

Strategy:
- Win-back Campaigns: Send special offers or reminders to re-engage them (e.g., "We miss you!" messages).
- Personalized Incentives: Offer discounts, coupons, or exclusive deals based on their past purchase behavior.
- Survey or Feedback: Ask them for feedback to understand why they’ve stopped purchasing or reduced frequency.
- Re-targeting Ads: Use digital ads to remind them of what they’re missing or offer a special promotion.

7. Churned Customers (R = 1-2, F = 1-2, M = 1-2)
- Description: Low recency, frequency, and monetary value.

Strategy:
- Reactivate with Strong Incentives: Offer significant discounts or promotions to try to bring them back.
- Feedback Loop: Reach out to understand their reason for leaving, offering solutions or discounts in return for feedback.
- Re-engagement Emails: Send targeted email campaigns with tailored offers or loyalty incentives.
- Personalized Recommendations: Encourage them with offers based on their past behavior and interests.

8. Other Segments (Mixed R, F, M)
- Description: Customers with a mixed set of recency, frequency, and monetary scores.

Strategy:
- Targeted Promotions: Identify specific patterns within these segments and create tailored campaigns.
- General Engagement: Keep the communication flow consistent with generic loyalty offers, newsletters, and promotions.
- Identify High Potentials: Use behavior tracking to move the most promising of these customers into more specific segments (e.g., Potential Loyalists or Loyal Customers).

By focusing on high-value customers (e.g., VIPs and Loyal Customers), you're able to optimize your marketing spend and improve customer retention. At the same time, you can actively work to bring back At Risk customers and transform them into Loyal ones.
