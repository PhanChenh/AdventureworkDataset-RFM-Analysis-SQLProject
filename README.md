# Projcet Title: RFM Analysis Using SQL on the AdventureWorks Dataset

## Project Overview: 

Dataset: AdventureWorks2022.bak 

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

Retrieves all columns and rows from the SalesOrderHeader table in the sales schema.
```sql
select * from sales.SalesOrderHeader;
```

Computed RFM metrics along with percentile-based segmentation for further analysis:
- Calculates RFM Metrics for each customerid
- Ranks Customers using Percentiles & Quintiles
```sql
select h. *,
	ntile(5) over (order by recency) as n_tile_recency,
	percentile_disc(0.2) within group (order by recency) over () as percent_20_recency,
	percentile_disc(0.4) within group (order by recency) over () as percent_40_recency,
	percentile_disc(0.6) within group (order by recency) over () as percent_60_recency,
	percentile_disc(0.8) within group (order by recency) over () as percent_80_recency,

	ntile(5) over (order by frequency) as n_tile_frequency,
	percentile_disc(0.2) within group (order by frequency) over () as percent_20_frequency,
	percentile_disc(0.4) within group (order by frequency) over () as percent_40_frequency,
	percentile_disc(0.6) within group (order by frequency) over () as percent_60_frequency,
	percentile_disc(0.8) within group (order by frequency) over () as percent_80_frequency,

	ntile(5) over (order by monetary) as n_tile_monetary,
	percentile_disc(0.2) within group (order by monetary) over () as percent_20_monetary,
	percentile_disc(0.4) within group (order by monetary) over () as percent_40_monetary,
	percentile_disc(0.6) within group (order by monetary) over () as percent_60_monetary,
	percentile_disc(0.8) within group (order by monetary) over () as percent_80_monetary

--into RFM_RawData

from (select customerid,
			datediff(day, max(OrderDate), '2014-01-01') recency,
			datediff(day, min(OrderDate), '2014-01-01') / count(*) frequency,
			sum(subtotal) / count(*) monetary
		from sales.SalesOrderHeader
		where year(OrderDate) = 2013
		group by customerid) as h
;
```

If we use the dataset from the code above to calculate RFM, it will calculate the RFM metrics only for 2013 orders up to January 1, 2014.
```sql
-- Declare variables for recency, frequency, and monetary percentiles
--- decare variables for recency
declare @percentile_20_r decimal(10,2);
declare @percentile_40_r decimal(10,2);
declare @percentile_60_r decimal(10,2);
declare @percentile_80_r decimal(10,2);

--- decare variables for frequency
declare @percentile_20_f decimal(10,2);
declare @percentile_40_f decimal(10,2);
declare @percentile_60_f decimal(10,2);
declare @percentile_80_f decimal(10,2);

--- decare variables for monetary
declare @percentile_20_m decimal(10,2);
declare @percentile_40_m decimal(10,2);
declare @percentile_60_m decimal(10,2);
declare @percentile_80_m decimal(10,2);

-- Fetch percentile values
select 
	@percentile_20_r = max(percent_20_recency),
	@percentile_40_r = max(percent_40_recency),
	@percentile_60_r = max(percent_60_recency),
	@percentile_80_r = max(percent_80_recency),


	@percentile_20_f = max(percent_20_frequency),
	@percentile_40_f = max(percent_40_frequency),
	@percentile_60_f = max(percent_60_frequency),
	@percentile_80_f = max(percent_80_frequency),

	@percentile_20_m = max(percent_20_monetary),
	@percentile_40_m = max(percent_40_monetary),
	@percentile_60_m = max(percent_60_monetary),
	@percentile_80_m = max(percent_80_monetary)

from RFM_RawData;

--GET SPECIFIC CUSTOMER IN SPECIFIC SEGMENT
-- Query customer segments using RFM analysis
WITH RFM_Scores AS (
    SELECT 
        customerid, 
        DATEDIFF(DAY, MAX(orderdate), '2014-01-01') AS recency,
        DATEDIFF(DAY, MIN(orderdate), '2014-01-01') / COUNT(*) AS frequency,
        SUM(subtotal) / COUNT(*) AS monetary,
        CASE 
            WHEN DATEDIFF(DAY, MAX(orderdate), '2014-01-01') <= @percentile_20_r THEN 1
            WHEN DATEDIFF(DAY, MAX(orderdate), '2014-01-01') > @percentile_20_r AND DATEDIFF(DAY, MAX(orderdate), '2014-01-01') <= @percentile_40_r THEN 2
            WHEN DATEDIFF(DAY, MAX(orderdate), '2014-01-01') > @percentile_40_r AND DATEDIFF(DAY, MAX(orderdate), '2014-01-01') <= @percentile_60_r THEN 3
            WHEN DATEDIFF(DAY, MAX(orderdate), '2014-01-01') > @percentile_60_r AND DATEDIFF(DAY, MAX(orderdate), '2014-01-01') <= @percentile_80_r THEN 4
            ELSE 5 
        END AS rb,
        CASE 
            WHEN DATEDIFF(DAY, MIN(orderdate), '2014-01-01') / COUNT(*) <= @percentile_20_f THEN 1
            WHEN DATEDIFF(DAY, MIN(orderdate), '2014-01-01') / COUNT(*) > @percentile_20_f AND DATEDIFF(DAY, MIN(orderdate), '2014-01-01') / COUNT(*) <= @percentile_40_f THEN 2
            WHEN DATEDIFF(DAY, MIN(orderdate), '2014-01-01') / COUNT(*) > @percentile_40_f AND DATEDIFF(DAY, MIN(orderdate), '2014-01-01') / COUNT(*) <= @percentile_60_f THEN 3
            WHEN DATEDIFF(DAY, MIN(orderdate), '2014-01-01') / COUNT(*) > @percentile_60_f AND DATEDIFF(DAY, MIN(orderdate), '2014-01-01') / COUNT(*) <= @percentile_80_f THEN 4
            ELSE 5 
        END AS fb,
        CASE 
            WHEN SUM(subtotal) / COUNT(*) <= @percentile_20_m THEN 1
            WHEN SUM(subtotal) / COUNT(*) > @percentile_20_m AND SUM(subtotal) / COUNT(*) <= @percentile_40_m THEN 2
            WHEN SUM(subtotal) / COUNT(*) > @percentile_40_m AND SUM(subtotal) / COUNT(*) <= @percentile_60_m THEN 3
            WHEN SUM(subtotal) / COUNT(*) > @percentile_60_m AND SUM(subtotal) / COUNT(*) <= @percentile_80_m THEN 4
            ELSE 5 
        END AS mb
    FROM sales.SalesOrderHeader
    WHERE YEAR(orderdate) = 2013
    GROUP BY customerid
)
-- Final segmentation based on RFM scores
SELECT customerid, recency, frequency, monetary, 
       rb * 100 + fb * 10 + mb AS rfm,
       CASE
           WHEN rb = 5 AND fb = 5 AND mb = 5 THEN 'VIP Customers'
           WHEN rb >= 4 AND fb >= 4 AND mb >= 4 THEN 'Loyal Customers'
           WHEN rb = 4 AND fb <= 3 AND mb <= 3 THEN 'Potential Loyalists'
           WHEN rb <= 3 AND fb >= 4 AND mb >= 4 THEN 'Big Spenders'
           WHEN rb = 3 AND fb <= 3 AND mb <= 3 THEN 'At Risk'
           WHEN rb = 1 AND fb <= 2 AND mb <= 2 THEN 'Churned Customers'
           WHEN rb = 3 AND fb = 4 AND mb <= 3 THEN 'Promising Customers'
           ELSE 'Other Segments'
       END AS RFM_Category
FROM RFM_Scores
ORDER BY rfm DESC; 
```

Code filter and select only the customers from the outer query who fall into specific category
```sql
-- Example: rfm = 555, VIP Customers
WHERE customerid IN (
    SELECT customerid
    FROM RFM_Scores
	WHERE rb = 5 AND fb = 5 AND mb = 5);
```

And this code is to filter for a specific segment (e.g., "Big Spenders") but also exclude customers in other segments like "VIP Customers", "Loyal Customers", and "Potential Loyalists."
```sql
WHERE customerid IN (
    SELECT customerid
    FROM RFM_Scores
    WHERE rb <= 3 AND fb >= 4 AND mb >= 4  -- Big Spenders condition (this can change to any segment you want to extract customerID)
)
AND NOT (rb = 5 AND fb = 5 AND mb = 5)  -- Exclude VIP Customers
AND NOT (rb >= 4 AND fb >= 4 AND mb >= 4) -- Exclude Loyal Customers
AND NOT (rb = 4 AND fb <= 3 AND mb <= 3) -- Exclude Potential Loyalists
ORDER BY customerid;
```

Clean up
```sql
DROP TABLE RFM_RawData;
```

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
