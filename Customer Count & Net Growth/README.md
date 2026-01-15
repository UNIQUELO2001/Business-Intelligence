# 📈Customer Count & Net Growth
# Objective
Built an accurate, repeatable system to track customer count, recurring services, and net customer growth. The goal was to establish a clear baseline, monitor daily performance, and make the highest-level KPI net new customers visible to the entire organization.

# Problem
The company lacked a consistent way to track customers over time. Historical data showed unexplained customer losses, with no reliable daily view of sales, churn, or total recurring customers.

# Solution
Designed a structured data workflow using GorillaDesk exports to track sales, churn, and active recurring services. Standardized definitions for recurring customers and services, validated counts weekly, and applied the formula:<br>

Old Count + Sales − Churn = New Count<br>

Built a centralized dashboard that visualized customer count, net growth, and progress toward targets, with daily visibility shared across the team.


# Data Pipeline
Data Source (BigQuery) > Data Connector (Google Sheets) > Formulas Google Sheets

# Tools and Skills
<ul>
  <li>Data analysis/li>
  <li>KPI design</li>
  <li>BigQuery SQL</li>
  <li>Reporting automation</li>
  <li>Looker Studio for dashboard design</li>
  <li>Business intelligence</li>
  <li>Zapier for Daily Slack update</li>
</ul>

# Workflow
Disclaimer: Sensitive data has been blurred to protect the company’s confidentiality

# 1) Extract your BigQuery preview data to Google sheets
<img width="1919" height="959" alt="sheets" src="https://github.com/user-attachments/assets/e4a6ec17-88c9-4630-b979-b5c69c90f374" />


# 2) Pull up the churn data in a different tab using this formula
```
=FILTER(
  {
    raw_data!A:A,
    raw_data!B:B,
    raw_data!C:C,
    raw_data!D:D,
    raw_data!F:F,
    raw_data!G:G,
    raw_data!J:J,
    raw_data!L:L,
    raw_data!E:E,
    raw_data!I:I
  },
  LOWER(raw_data!J:J) = "terminate service",
  REGEXMATCH(
    LOWER(raw_data!C:C),
    "commercial service plan|mosquito service plan|pest control basic service plan|pest control enhanced service plan|pest control premium service plan|pest control service plan|rodent control service plan|termite baiting service plan|termite liquid service plan|termite wood treatment service plan"
  )
)

```
<img width="1862" height="912" alt="sheets2" src="https://github.com/user-attachments/assets/d73ee70b-6a73-4c1a-a7c7-9cd820a4909c" />

# 3) Sales data is pulled up from a different sheet using "QUERY" and "IMPORTRANGE" 
```
=QUERY({
  IMPORTRANGE("https://docs.google.com/spreadsheets/", "Data!A:V");
  QUERY(IMPORTRANGE("https://docs.google.com/spreadsheets/", "Data!A:V"), "select * offset 1", 0);
  QUERY(IMPORTRANGE("https://docs.google.com/spreadsheets/", "Data!A:V"), "select * offset 1", 0)
},
"select * where Col1 is not null", 1)
```
<img width="1918" height="952" alt="sheets3" src="https://github.com/user-attachments/assets/6199d25e-5848-45f5-b936-3d79109c7caf" />

# 4) Build a lookup sheet for a summary of data and to set up Zapier Automation
<img width="1919" height="959" alt="sheet4" src="https://github.com/user-attachments/assets/b20d13ac-cffb-47bf-af47-c01fb6c2beda" />

Using these formulas for data transformation and cleaning
```
=ARRAYFORMULA(
  IF( ROW(A2:A) - ROW(A2) + 1 <= TODAY() - DATE(2024,1,1),
      DATE(2024,1,1) + ROW(A2:A) - ROW(A2),
      ""
  )
)
```
```
=ARRAYFORMULA(
  IF(A2:A="","",
    COUNTIFS(
      sales_data!Q$2:Q, A2:A,
      sales_data!V$2:V, "Austin",
      sales_data!B$2:B, "<>TRUE",
      sales_data!O$2:O, "<>0"     
    )
  )
)
```
```
=ARRAYFORMULA(
  IF(A2:A="","",
    COUNTIFS(
      churn_data!F$2:F, A2:A,
      churn_data!I$2:I, "Austin"      
    )
  )
)
```
```
=D2 + IF(A3="", 0, B3 - C3)
```
```
=ARRAYFORMULA(
  IF(A2:A="", "", B2:B- C2:C)
)
```
```
=ARRAYFORMULA(
  IF(A2:A="", "", IF(ISNUMBER(E2:E + I2:I), E2:E + I2:I, ""))
)
```
```
=ARRAYFORMULA(
  IF(A2:A = "",
     "",
     TEXT(A2:A, "MM/DD/YYYY") = TEXT(TODAY() - 1, "MM/DD/YYYY")
  )
)
```
```
=ARRAYFORMULA(
  IF(A2:A="", "", D2:D+ H2:H)
)
```
```
=ARRAYFORMULA(UNIQUE(FILTER(A2:A - WEEKDAY(A2:A, 1) + 1, A2:A<>"")))
```
```
=ARRAYFORMULA(IF(N2:N="",,SUMIF(A2:A - WEEKDAY(A2:A,1) + 1, N2:N, B2:B)))
```
```
=ARRAYFORMULA(IF(N2:N="",,SUMIF(A2:A - WEEKDAY(A2:A,1) + 1, N2:N, C2:C)))
```

# 5) Build simple dashboard in Sheets for the summarized data
<img width="1920" height="959" alt="sheets5" src="https://github.com/user-attachments/assets/f07b314c-771f-44f9-a84a-c795845e3289" />

# 6) Connect data to Looker Studio for visualization
Sales Dashboard
<img width="960" height="828" alt="dashboard" src="https://github.com/user-attachments/assets/4147f441-f25d-4b57-94ca-19ca73323f56" />
<img width="960" height="699" alt="dashboard2" src="https://github.com/user-attachments/assets/57eb913f-8038-43c9-9fd6-4a39177e0d4b" />
<img width="961" height="383" alt="dashboard3" src="https://github.com/user-attachments/assets/b1203036-0069-4a30-8ffd-a1486adf000d" />

Recurring Services Dashboard
<img width="958" height="825" alt="dashboard" src="https://github.com/user-attachments/assets/111b9505-5615-4fb8-9e51-9c1a31f94328" />
<img width="962" height="701" alt="dashboard2" src="https://github.com/user-attachments/assets/15cdeda9-3b01-459c-91ec-32b59dbb8b8f" />
<img width="963" height="348" alt="dashboard3" src="https://github.com/user-attachments/assets/0e0a15e4-b05e-4fd0-a181-d78a37387c6f" />

Churn Dashboard
<img width="956" height="833" alt="dashboard" src="https://github.com/user-attachments/assets/f9bf5b8d-6a52-4a41-967f-30ea010dc6ff" />
<img width="950" height="700" alt="dashboard2" src="https://github.com/user-attachments/assets/dbecf807-0810-43ed-ac38-199aed67233f" />
<img width="958" height="341" alt="dashboard3" src="https://github.com/user-attachments/assets/0722b12d-0e19-4ce6-98d8-432987a00a91" />

# 5) Create daily update with Zapier and test with Slack
<img width="402" height="585" alt="zapier" src="https://github.com/user-attachments/assets/56a5aa08-3ac6-47ba-b523-3f53c5c24108" />
<img width="282" height="342" alt="slack" src="https://github.com/user-attachments/assets/c299c3a6-247a-4f42-8d78-90c94a18f6ab" />

# Results and Impact
<ul>
  <li>Established a trusted customer count benchmark</li>
  <li>Enabled daily visibility of net customer growth</li>
  <li>Improved accountability around sales and churn</li>
  <li>Supported a 20% growth target with clear, measurable KPIs</li>
  <li>Delivered dashboards that aligned management and frontline teams</li>
</ul>
