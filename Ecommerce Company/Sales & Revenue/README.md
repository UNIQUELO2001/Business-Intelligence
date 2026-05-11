# 💰 Sales & Revenue Dashboard

# Objective
Develop a centralized Sales & Revenue dashboard to monitor company sales performance, revenue trends, transaction behavior, and branch-level performance in real time, enabling management to make faster and more accurate business decisions.

# Problem
The company lacked a unified reporting system for monitoring sales and revenue performance across branches and departments. Reports were manually consolidated from multiple spreadsheets, resulting in delayed insights, inconsistent calculations, reporting errors, and limited visibility into sales trends and business performance.

# Solution
Create an automated Sales & Revenue dashboard that consolidates transactional and operational sales data into a centralized reporting system. The dashboard provides real-time visibility into revenue performance, branch comparisons, transaction trends, growth analysis, and sales KPIs through interactive and dynamic visualizations.

# Data Pipeline
Data are extracted from sales systems, transaction records, operational spreadsheets, and branch reports > Data is transformed and cleaned inside Google BigQuery using SQL queries > Processed datasets are connected to Google Looker Studio > KPI metrics and interactive dashboards are developed for executive and operational reporting

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

# Workflow
> **Disclaimer:** The data presented in this demonstration uses dummy/sample data for confidentiality and company data protection purposes. Only the reporting structure, dashboard framework, calculations, and analytical design are showcased. No actual company data, financial figures, customer information, or operational records are displayed.

# 1) SQL Queries Used

## Daily Revenue Query
```sql
SELECT
  DATE(transaction_date) AS sales_date,
  branch,
  SUM(total_amount) AS total_revenue,
  COUNT(DISTINCT transaction_id) AS total_transactions
FROM `project_id.dataset.sales_transactions`
GROUP BY sales_date, branch
ORDER BY sales_date ASC;
```

## Monthly Sales Trend Query
```sql
SELECT
  FORMAT_DATE('%Y-%m', DATE(transaction_date)) AS sales_month,
  SUM(total_amount) AS monthly_revenue
FROM `project_id.dataset.sales_transactions`
GROUP BY sales_month
ORDER BY sales_month ASC;
```

## Branch Sales Performance Query
```sql
SELECT
  branch,
  SUM(total_amount) AS total_revenue,
  COUNT(DISTINCT transaction_id) AS total_transactions,
  AVG(total_amount) AS average_transaction_value
FROM `project_id.dataset.sales_transactions`
GROUP BY branch
ORDER BY total_revenue DESC;
```

## Top Products Query
```sql
SELECT
  product_name,
  SUM(quantity) AS total_units_sold,
  SUM(total_amount) AS total_sales
FROM `project_id.dataset.sales_transactions`
GROUP BY product_name
ORDER BY total_sales DESC
LIMIT 10;
```

# 2) Formulas used

## Total Revenue
```sql
SUM(total_amount)
```

## Total Transactions
```sql
COUNT(DISTINCT transaction_id)
```

## Average Transaction Value
```sql
SAFE_DIVIDE(SUM(total_amount), COUNT(DISTINCT transaction_id))
```

## Sales Growth Rate
```sql
SAFE_DIVIDE(
  (current_period_sales - previous_period_sales),
  previous_period_sales
)
```

## Revenue Contribution Percentage
```sql
SAFE_DIVIDE(branch_revenue, total_company_revenue) * 100
```

# 3) Final Product

<img width="600" height="850" alt="{A64D19E8-A213-4486-AB23-A710EC067A56}" src="https://github.com/user-attachments/assets/6f995509-1f99-4a2b-9ba6-352d767b316d" />
<img width="603" height="709" alt="{929100CB-D246-4219-8AAA-FF7CA03BBCD8}" src="https://github.com/user-attachments/assets/634ce79a-e864-4529-b8a9-afafda7af64c" />
<img width="603" height="664" alt="{FE9824A1-299F-4849-9468-9B56BB408DA1}" src="https://github.com/user-attachments/assets/01d3ab5d-2a94-42a5-b0ad-e5b079cf3eaa" />



# Results and Impact
<ul>
  <li>Centralized sales and revenue reporting across all branches</li>
  <li>Reduced manual reporting and spreadsheet consolidation</li>
  <li>Improved visibility into daily, weekly, and monthly sales trends</li>
  <li>Enabled faster identification of high-performing and underperforming branches</li>
  <li>Improved revenue forecasting and sales monitoring</li>
  <li>Provided real-time executive visibility into company revenue performance</li>
  <li>Improved operational decision-making through automated analytics</li>
  <li>Established a single source of truth for sales reporting</li>
</ul>

> **Disclaimer:** The data presented in this demonstration uses dummy/sample data for confidentiality and company data protection purposes. Only the reporting structure, dashboard framework, calculations, and analytical design are showcased. No actual company data, financial figures, customer information, or operational records are displayed.
