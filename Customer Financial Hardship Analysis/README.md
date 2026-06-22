**Customer Financial Hardship Analysis**

## Objective
Analyze percentages of charge off accounts that are associated with customers losing their jobs during covid.

## Business Problem

American First Finance wanted to understand the impact of customer financial hardship on portfolio losses. Leadership specifically wanted visibility into how often unemployment was associated with charged-off accounts.

**Approach**
Analyzed customer account notes for indicators of financial hardship.
Identified references to unemployment and job loss using keyword searches.
Linked customer note activity to charge-off account balances.
Quantified the relationship between unemployment indicators and portfolio losses.

## Skills Demonstrated

- SQL
- Text Analysis
- Data Mining
- Root Cause Analysis
- Financial Analytics
- Portfolio Risk Analysis
- KPI Reporting
- Business Intelligence

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


