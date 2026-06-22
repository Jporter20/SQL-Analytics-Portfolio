**Collections Portfolio Monitoring**


## Objective
Analyze percentages of charge off accounts that are associated with customers losing their jobs during covid.

**Business Problem**

American First Finance wanted to understand the percentages of deliquent account in each deliquent bucket for daily and monthly reporting. Leadership specifically wanted visibility into what percentage of customers fell into each deliquency bucket.

## Analysis Performed
- Total deliquencies in each deliquency bucket
- Trend analysis
- MTD changes
- 
## Key Insights
- Identified top deliquency buckets with the highest percentage
- Track deliquency bucket percentages daily and MTD
- Provided actionable recommendations
  
## Skills Demonstrated

- SQL
- Data Mining
- Financial Analytics
- Portfolio Risk Analysis
- KPI Reporting
- Business Intelligence

## Sample SQL Logic

```sql

**Collections BUCKET % AND TOTALS**
# % of portfolio in each bucket for daily and monthly reporting. This bucket analysis was to assist marketing with direction on which bucket category of customers should be targeted in promotions for repayment.

**Daily**
select date,
	   co_status,
       ROUND(sum(current_balance),2) AS current_balance_total,
       ROUND(sum(current_balance),2)/ (SELECT ROUND(sum(current_balance),2) from portfolio where date = '2019-11-08' and co_status <> 'CO') * 100 AS percentage
from collections
where date = '2019-11-08'
and status <> 'CO'
GROUP BY co_status

**MTD**
select date
	   -- ,co_status
       ,ROUND(sum(current_balance),2) AS current_balance_total
       ,SUM(CASE WHEN co_status = 0 THEN current_balance ELSE 0 END)/ sum(current_balance) * 100 AS status_0_balance
       ,SUM(CASE WHEN co_status = 1 THEN current_balance ELSE 0 END)/ sum(current_balance) * 100 AS status_1_balance
       ,SUM(CASE WHEN co_status = 2 THEN current_balance ELSE 0 END)/ sum(current_balance) * 100 AS status_2_balance
       ,SUM(CASE WHEN co_status = 3 THEN current_balance ELSE 0 END)/ sum(current_balance) * 100 AS status_3_balance
       ,SUM(CASE WHEN co_status = 4 THEN current_balance ELSE 0 END)/ sum(current_balance) * 100 AS status_4_balance
       -- ,ROUND(sum(current_balance),2)/ (SELECT sum(current_balance) from a_portfolio_snapshot where snapshot_date > DATE_FORMAT(CURDATE(), '%Y-%m-01' and status <> 'CO') * 100 AS percentage
from collections
where date > DATE_FORMAT(CURDATE(), '%Y-%m-01')
and co_status <> 'CO'
GROUP BY date


```

