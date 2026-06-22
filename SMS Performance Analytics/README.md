**SMS Performance Analytics**

**Business Problem**

Leadership lacked visibility into customer SMS communication performance and wanted to understand how effectively inbound customer messages were being handled.

The objective was to measure inbound SMS volume, response rates, and average response times to identify operational trends and improve customer communication performance.

**Approach**
Collected inbound and outbound SMS activity.
Matched customer messages with corresponding agent responses.
Calculated response rates and response times.
Aggregated results by date and hour to identify communication trends throughout the day.
Created reporting metrics to provide leadership visibility into SMS channel performance.

**Tools Used**
SQL
Excel
Data Visualization
KPI Reporting
Analysis Performed
Inbound SMS volume tracking
Hourly communication analysis
Response rate calculations
Average response time calculations
Peak activity identification
Operational performance monitoring

**Key Insights**
Identified peak customer communication hours.
Measured the percentage of customer messages receiving responses.
Calculated average response times across reporting periods.
Highlighted opportunities to improve responsiveness during high-volume periods.
Provided leadership with visibility into SMS channel performance and customer engagement trends.

**Skills Demonstrated**
SQL
KPI Reporting
Data Analysis
Operational Analytics
Root Cause Analysis
Data Visualization
Business Intelligence
Process Improvement
Business Impact

This analysis provided operational leadership with visibility into customer communication effectiveness and helped identify opportunities to improve response rates and reduce customer wait times.

**Example Business Questions Answered**
How many inbound customer messages are received each day?
What percentage of messages receive responses?
What is the average response time?
During which hours is SMS activity highest?
Are customer response times improving or declining over time?

## Sample SQL Logic

```sql

 -- HOURLY SMS
    SELECT sms.sent_date, HOUR(sms.sent_time) as HOUR, COUNT(*) AS total_inbound_volume,
          count(outbound_date_time) as total_responded_to,
          MAX(outbound_date_time) AS maximum_response,
          SEC_TO_TIME(AVG(TIME_TO_SEC(difference))) AS avg_response_time
FROM (SELECT DISTINCT cs.sent_date, cs.sent_time, cs.source_number, 
                        CONCAT(cs.sent_date, ' ', cs.sent_time) as inbound_date_time, 
                        CONCAT(outb.sent_date, ' ', outb.sent_time) as outbound_date_time,
                        CASE WHEN CONCAT(cs.sent_date, ' ', cs.sent_time) >= CONCAT(outb.sent_date, ' ', outb.sent_time) THEN ABS(TIMEDIFF(CONCAT(outb.sent_date, ' ', outb.sent_time), CONCAT(cs.sent_date, ' ', cs.sent_time)))
                             WHEN CONCAT(outb.sent_date, ' ', outb.sent_time) >= CONCAT(cs.sent_date, ' ', cs.sent_time) THEN ABS(TIMEDIFF(CONCAT(outb.sent_date, ' ', outb.sent_time), CONCAT(cs.sent_date, ' ', cs.sent_time)))
                        ELSE ' '
                        END as difference, 
                        cs.message as inbound_message, outb.outbound_message
       FROM call_center cs
       LEFT JOIN
              (SELECT *
              FROM call_center sms
              WHERE HOUR(sent_time) between 8 and 19
              GROUP BY destination_number, sent_date ORDER BY destination_number, sent_date, sent_time) outb 
       on cs.source_number = outb.destination_number
        and cs.sent_date = outb.sent_date
       WHERE HOUR(cs.sent_time) between 8 and 19
       GROUP BY cs.sent_date, cs.source_number) sms
    GROUP BY sms.sent_date, HOUR


```
