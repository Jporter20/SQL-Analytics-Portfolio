**Customer Financial Hardship Analysis**

## Objective
Analyze percentages of charge off accounts that are associated with customers losing their jobs during covid.

## Analysis Performed
- Total deliquencies in each deliquency bucket
- Trend analysis
- MTD changes

## Key Insights
- Identified top deliquency buckets with the highest percentage
- Track deliquency bucket percentages daily and MTD
- Provided actionable recommendations

## Sample SQL Logic

```sql

SELECT
    COUNT(DISTINCT n.customerID) AS unemployed_customers,
    SUM(l.charge_off_balance) AS charge_off_balance
FROM notes n
INNER JOIN loan l
    ON n.customerID = l.customerID
WHERE (
        n.notes LIKE '%lost job%'
        OR n.notes LIKE '%unemployed%'
      )
  AND n.date >= CURDATE() - INTERVAL 3 MONTH;

```


