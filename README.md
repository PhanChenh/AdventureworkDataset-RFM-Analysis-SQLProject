# Projcet Title: RFM Analysis Using SQL on the AdventureWorks Dataset

## Project Overview: 

Dataset: AdventureWorks2022.bak 

RFM analysis is a basic model for customer segmentation or customer segmentation based on customer buying behavior. By doing this, you gain insights into which customers are the most valuable to your business, how recently they interacted with your business, how often they make purchases, and how much they spend.

## Objectives:

## Project Structure:
1. Identify Customer Segments:
- Recency (R): How recently did the customer make a purchase? Customers who 
bought recently are more likely to buy again.
- Frequency (F): How often does the customer make a purchase? Frequent buyers are often more loyal and engaged.
- Monetary (M): How much money does the customer spend? High spenders are more profitable.

2. Focus on the Right Marketing Tactics:
By analyzing each segment’s behavior, you can direct marketing efforts where 
they will be most effective.

---

```sql
select * from sales.SalesOrderHeader;
```

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

```sql
-- if we use the dataset from the code above to calculate RFM, it will calculate the RFM metrics only for 2013 orders up to January 1, 2014.
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

```sql
--Cleanup
DROP TABLE RFM_RawData;
```
