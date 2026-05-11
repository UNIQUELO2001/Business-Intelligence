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
  <li>Power BI</li>
  <li>SQL</li>
  <li>Excel Power Query</li>
  <li>DAX</li>
  <li>Power Pivot</li>
  <li>Data Modeling</li>
  <li>ETL Development</li>
  <li>Dashboard Design</li>
  <li>Business Analysis</li>
  <li>Data Visualization</li>
</ul>

# Workflow
Disclaimer: Sensitive company data and confidential business information have been blurred or excluded to protect company confidentiality.

# 1) SQL Queries Used

## Revenue Aggregation Query
```sql
SELECT
    branch_name,
    DATE(transaction_date) AS sales_date,
    SUM(quantity * unit_price) AS total_revenue
FROM sales_transactions
WHERE transaction_date BETWEEN '2025-01-01' AND '2025-12-31'
GROUP BY branch_name, DATE(transaction_date)
ORDER BY sales_date ASC;
```

## Branch Performance Query
```sql
SELECT
    branch_name,
    COUNT(transaction_id) AS total_transactions,
    SUM(total_amount) AS revenue,
    AVG(total_amount) AS average_transaction_value
FROM sales_transactions
GROUP BY branch_name
ORDER BY revenue DESC;
```

## Inventory Movement Query
```sql
SELECT
    product_name,
    SUM(stock_in) AS total_stock_in,
    SUM(stock_out) AS total_stock_out,
    SUM(current_stock) AS remaining_inventory
FROM inventory_records
GROUP BY product_name;
```

## Employee Productivity Query
```sql
SELECT
    employee_name,
    branch_name,
    COUNT(transaction_id) AS transactions_processed,
    SUM(total_amount) AS total_sales
FROM sales_transactions
GROUP BY employee_name, branch_name
ORDER BY total_sales DESC;
```

# 2) Formulas used

## Revenue
```DAX
Revenue =
SUMX(
    Sales,
    Sales[Quantity] * Sales[Unit Price]
)
```

## Gross Profit Margin
```DAX
Gross Profit Margin =
DIVIDE(
    [Revenue] - [COGS],
    [Revenue]
) * 100
```

## Sales Growth Rate
```DAX
Sales Growth Rate =
DIVIDE(
    [Current Period Sales] - [Previous Period Sales],
    [Previous Period Sales]
) * 100
```

## Inventory Turnover
```DAX
Inventory Turnover =
DIVIDE(
    [COGS],
    [Average Inventory]
)
```

## Average Transaction Value
```DAX
Average Transaction Value =
DIVIDE(
    [Total Revenue],
    [Number of Transactions]
)
```

# 3) Final Product

<img width="1910" height="904" alt="Executive Dashboard" src="https://github.com/user-attachments/assets/sample-dashboard-overview.png" />

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
