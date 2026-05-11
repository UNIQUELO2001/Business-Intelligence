# 📊 Executive Overview Dashboard

# Objective
Develop a centralized executive overview dashboard that provides leadership with real-time visibility into company-wide operational and financial performance, enabling faster, data-driven decisions across multiple business units.

# Problem
The company relied on manually consolidated Excel reports from different departments, causing delayed reporting, inconsistent KPIs, human errors, and limited visibility into business performance. Executives spent more time validating reports instead of making strategic decisions due to fragmented data sources and lack of centralized reporting.

# Solution
Build a fully automated executive dashboard that consolidates operational and financial data into a single source of truth. The dashboard standardizes KPIs, automates reporting processes, and provides interactive analytics for monitoring revenue, profitability, operational efficiency, inventory movement, and branch performance.

# Data Pipeline
Data are extracted from POS systems, ERP databases, inventory management systems, HR attendance records, and operational spreadsheets using SQL queries > Data is cleaned and transformed using SQL and Power Query > KPI calculations are developed using DAX and formulas > Processed datasets are modeled into Power BI for executive reporting and automated refreshes

# Tools and Skills
<ul>
  <li>Google BigQuery</li>
  <li>Google Looker Studio</li>
  <li>SQL</li>
  <li>Google Sheets</li>
  <li>Data Cleaning</li>
  <li>Data Modeling</li>
  <li>ETL Development</li>
  <li>KPI Development</li>
  <li>Dashboard Design</li>
  <li>Business Analysis</li>
  <li>Data Visualization</li>
</ul>

# Data Pipeline
Data are extracted from operational sources such as sales records, customer activity, production reports, utilization sheets, cancellations, and missed calls > Data is processed and transformed in Google BigQuery using SQL queries > Cleaned and modeled tables are connected to Google Looker Studio > Executive KPIs and visual reports are built in Looker Studio for centralized performance monitoring

# 1) SQL Queries Used

## Revenue Summary Query
```sql
SELECT
  DATE(transaction_date) AS sales_date,
  branch,
  SUM(total_amount) AS total_revenue,
  COUNT(DISTINCT transaction_id) AS total_transactions
FROM `project_id.dataset.sales_table`
GROUP BY sales_date, branch
ORDER BY sales_date ASC;
```

## Branch Performance Query
```sql
SELECT
  branch,
  SUM(total_amount) AS revenue,
  COUNT(DISTINCT transaction_id) AS total_transactions,
  AVG(total_amount) AS average_transaction_value
FROM `project_id.dataset.sales_table`
GROUP BY branch
ORDER BY revenue DESC;
```

## KPI Overview Query
```sql
SELECT
  report_date,
  SUM(sales) AS total_sales,
  SUM(cancellations) AS total_cancellations,
  SUM(missed_calls) AS total_missed_calls,
  SUM(production_count) AS total_production,
  AVG(utilization_rate) AS average_utilization_rate
FROM `project_id.dataset.executive_kpi_table`
GROUP BY report_date
ORDER BY report_date ASC;
```

## Utilization Query
```sql
SELECT
  report_date,
  team,
  SAFE_DIVIDE(SUM(productive_hours), SUM(available_hours)) AS utilization_rate
FROM `project_id.dataset.utilization_table`
GROUP BY report_date, team
ORDER BY report_date ASC;
```

# 2) Formulas used

## Revenue
```sql
SUM(total_amount)
```

## Average Transaction Value
```sql
SAFE_DIVIDE(SUM(total_amount), COUNT(DISTINCT transaction_id))
```

## Cancellation Rate
```sql
SAFE_DIVIDE(SUM(cancellations), SUM(total_sales))
```

## Missed Call Rate
```sql
SAFE_DIVIDE(SUM(missed_calls), SUM(total_calls))
```

## Utilization Rate
```sql
SAFE_DIVIDE(SUM(productive_hours), SUM(available_hours))
```

# 3) Final Product

<img width="600" height="762" alt="{5235F2FF-C86A-4706-9138-5D3328C64CF0}" src="https://github.com/user-attachments/assets/31ca9767-5563-4afe-b949-2b271105999c" />
<img width="599" height="869" alt="{B2B54D61-6D18-4243-8C78-14B6E08275B2}" src="https://github.com/user-attachments/assets/2edff5de-313e-48b3-9de1-d06e3d48c638" />

# Results and Impact
<ul>
  <li>Reduced manual reporting workload by 80%</li>
  <li>Reporting preparation time reduced from several days to under 30 minutes</li>
  <li>Improved visibility into branch and company-wide performance</li>
  <li>Standardized KPI reporting across departments</li>
  <li>Enabled faster executive decision-making through real-time analytics</li>
  <li>Improved operational accountability and performance tracking</li>
  <li>Early identification of sales decline and operational inefficiencies</li>
  <li>Created a centralized single source of truth for business reporting</li>
</ul>

> **Disclaimer:** The data presented in this demonstration uses dummy/sample data for confidentiality and company data protection purposes. Only the reporting structure, dashboard framework, calculations, and analytical design are showcased. No actual company data, financial figures, customer information, or operational records are displayed.
