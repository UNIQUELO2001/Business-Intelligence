# 💯Company KPIs
# Objective
Identify and define the company’s core KPIs to create a clear, consistent view of performance, enable data-driven decision-making, and ensure alignment with growth, retention, and operational goals.

# Problem
Without clearly defined KPIs, the company lacks visibility into performance, relies on assumptions instead of data, struggles to align teams around shared goals, and cannot accurately measure progress, risks, or the impact of strategic decisions.

# Solution
Establish a clear set of company-wide KPIs with standard definitions, data sources, and reporting cadence to create a single source of truth, align teams around measurable goals, and enable consistent, data-driven decisions.

# Data Pipeline
Datas are pulled from sales, cancellations, missed calls, production, and utilization sheets  > Identify KPI using formulas on Google Sheets

# Tools and Skills
<ul>
  <li>Google Sheets/li>
  <li>Business Insights</li>
  <li>Data Analysis</li>
</ul>

# Workflow
Disclaimer: Sensitive data has been blurred to protect the company’s confidentiality

# 1) Formulas used
```
=TRANSPOSE(
  FILTER(
    INDEX(IMPORTRANGE("https://googlesheets.com","'summary_sheet'!O2:O")),
    INDEX(IMPORTRANGE("https://googlesheets.com","'summary_sheet'!N2:N"))>=DATE(2024,12,29),
    INDEX(IMPORTRANGE("https://googlesheets.com","'summary_sheet'!N2:N"))<DATE(2026,1,1)
  )
)
```
```
=TRANSPOSE(
  ARRAYFORMULA(
    IF(
      LEN(
        INDEX(
          IMPORTRANGE("https://docs.google.com/spreadsheets/","lookup!A2:T"),
        ,19)
      )=0,
      ,
      INDEX(
        IMPORTRANGE("https://docs.google.com/spreadsheets/d/","lookup!A2:T"),
      ,19)
    )
  )
)

```
```
=SPARKLINE()
```
# 2) Final Product
<img width="1910" height="904" alt="ss" src="https://github.com/user-attachments/assets/308e9609-5770-483f-9a88-35a734f22e3f" />

# Results and Impact
<ul>
  <li>Clear visibility into company performance and trends</li>
  <li>Alignment across teams around shared, measurable goals</li>
  <li>Faster and more confident decision-making</li>
  <li>Improved accountability and ownership of outcomes/li>
  <li>Stronger ability to track growth, retention, and operational efficiency</li>
</ul>
