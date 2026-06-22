**Call Center KPI Reporting**

## Sample SQL Logic

```sql
SELECT call_date,
       HOUR(call_start_time) AS hour,
       COUNT(call_date) AS calls_offered,
       SUM(operator_transfer_successful) AS calls_handled,
       SUM(operator_transfer_successful) / COUNT(operator_transfer) AS handled_rate,
       SUM(CASE WHEN operator_transfer_successful = 0 THEN 1 ELSE 0 END) AS abandoned_calls,
       SUM(CASE WHEN operator_transfer_successful = 0 THEN 1 ELSE 0 END) / COUNT(operator_transfer) AS abandonment_rate,
       SUM(CASE WHEN operator_transfer_successful = 1 THEN hold_duration ELSE 0 END) / SUM(operator_transfer_successful) AS asa_seconds
FROM outbound_dialer
WHERE operator_transfer = 1
  AND call_date >= '2019-01-01'
GROUP BY call_date, HOUR
ORDER BY call_date, HOUR;
```
## Objective
This query was used to analyze call center performance by hour, calculating offered calls, handled calls, abandonment rates, and average speed of answer (ASA).

## Analysis Performed
- Analyze call center performance for 3 call centers 
- Trend analysis of call center KPIs
- Agent statistics

## Key Insights
- Identified call volume trends
- Track agent KPIs & metrics
- Provided actionable recommendations
