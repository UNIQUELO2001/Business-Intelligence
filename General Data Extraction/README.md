# 📊General Data Extraction
# Objective
Automated data extraction and reporting pipeline for daily performance tracking.

# Problem
GorillaDesk does not have the feature to extract data with unique IDs, which was a problem since the datas are updating daily, this creates duplicates and gives the client a hard time getting accurate data.

# Solution
Use complex data ingestion and queries to assign unique IDs to each job/work order.

# Data Pipeline
Data Source (GorillaDesk) > Data Ingestion (Google Colab) > Data Storage and Cleaning (BigQuery) > Preview (Google Sheets)
# Steps:
<ol>
  <li>Extract data from GorillaDesk</li>
  <li>Ingest Data to Python (Google Colab)</li>
  <li>Store and clean data with BigQuery</li>
  <li>Preview the cleaned data to Google Sheets</li>
</ol>

# Architecture Overview
<img width="1920" height="1080" alt="GorillaDesk (Data Source)" src="https://github.com/user-attachments/assets/8702f4b9-1da0-43c9-9daf-6a4df8fe8fbe" />

# Tools and Skills
<ul>
  <li>Python</li>
  <li>BigQuery</li>
  <li>ELT pipelines</li>
  <li>Data cleaning and transformation</li>
  <li>Automated reporting</li>
</ul>

# Workflow
Disclaimer: Sensitive data has been blurred to protect the company’s confidentiality

# 1) Ingest data from GorillaDesk to Google Colab
``` python
from google.colab import files
uploaded = files.upload()
```
``` python
#LOADS THE FILE UPLOADED
import pandas as pd

filename = list(uploaded.keys())[0]
df = pd.read_csv(filename)

df.head()
df.shape
```
``` python
#SANITIZE COLUMN
import re

def sanitize_column(col):
    col = col.strip().lower()
    col = col.replace("#", "number")
    col = col.replace("/", " ")
    col = col.replace("%", "percent")
    col = re.sub(r"[^\w\s]", "", col)
    col = re.sub(r"\s+", "_", col)
    return col

df.columns = [sanitize_column(c) for c in df.columns]

df.columns
```
``` python
#DEFINE IDENTITY FIELDS
identity_cols = [
    "account_number",
    "service_type",
    "service_address_1",
    "service_no",
    "branch"
]
```
``` python
#Normalize Identity Values
def normalize(val):
    if pd.isna(val):
        return ""
    return str(val).strip().lower()

for col in identity_cols:
    df[col] = df[col].apply(normalize)
```
``` python
#GENERATE JOB KEY
import hashlib

def generate_job_key(row):
    key = "|".join(row[col] for col in identity_cols)
    return hashlib.sha256(key.encode("utf-8")).hexdigest()

df["job_instance_key"] = df.apply(generate_job_key, axis=1)
```
``` python
#CHECK IF JOB KEY GENERATED (Must be Null)
df[["job_instance_key"]].head()
df["job_instance_key"].isna().sum()
```
``` python
#ADDS INGESTED DATE AND TIME
from datetime import datetime, date

df["ingestion_date"] = date.today()
df["ingestion_timestamp"] = datetime.utcnow()
df["source_file"] = filename
```
``` python
#SHOWS HASH ROW FOR THE JOBS
df["row_hash"] = (
    df.astype(str)
      .agg("|".join, axis=1)
      .apply(lambda x: hashlib.sha256(x.encode("utf-8")).hexdigest())
)
```
This assigns unique identifiers for each row.
The combination keys to determine their IDs are: Customer Name, Account Number, Service Type, Service Address, and service number

# 2) Upload the ingested data to BigQuery for storage and transformation
``` python
from google.colab import auth
auth.authenticate_user()
```
``` python
from google.cloud import bigquery

PROJECT_ID = "general-data"
DATASET_ID = "idsample"
TABLE_ID = "raw_data_table"

FULL_TABLE_ID = f"{PROJECT_ID}.{DATASET_ID}.{TABLE_ID}"

client = bigquery.Client(project=PROJECT_ID)
```
``` python
#FOR FIRST TIME USE: "write_disposition="WRITE_TRUNCATE""
#FOR FUTURE USE (USE IT NOW): "write_disposition="WRITE_APPEND""
job_config = bigquery.LoadJobConfig(
    write_disposition="WRITE_APPEND"  # create or replace
)

load_job = client.load_table_from_dataframe(
    df,
    FULL_TABLE_ID,
    job_config=job_config
)

load_job.result()
```

# 3) BigQuery automatically updates the data 
``` sql
CREATE OR REPLACE VIEW gorilladesk.vw_jobs_current AS
SELECT
  -- all columns except raw date strings
  * EXCEPT (scheduled_date, last_service_date),

  -- scheduled_date as DATE
  SAFE.PARSE_DATE(
    '%m/%d/%Y',
    REGEXP_EXTRACT(scheduled_date, r'^(\d{2}/\d{2}/\d{4})')
  ) AS scheduled_date,

  -- last_service_date as DATE
  SAFE.PARSE_DATE(
    '%m/%d/%Y',
    REGEXP_EXTRACT(last_service_date, r'^(\d{2}/\d{2}/\d{4})')
  ) AS last_service_date

FROM (
  SELECT
    *,
    ROW_NUMBER() OVER (
      PARTITION BY job_instance_key
      ORDER BY ingestion_timestamp DESC
    ) AS rn
  FROM gorilladesk.raw_gorilladesk_jobs
)
WHERE rn = 1;
```
This query overwrites the updated rows
``` sql
SELECT
  customer,
  account_number,
  service_type,
  scheduled_date,
  last_service_date,
  job_status,
  branch
FROM gorilladesk.vw_jobs_current
LIMIT 10;
```
Display the Query
<img width="1636" height="601" alt="display" src="https://github.com/user-attachments/assets/91e98858-8f24-4e8d-a220-9fdbf53f993b" />

# 4) Connect data from BigQuery to Google Sheets
<img width="1918" height="955" alt="gsheets" src="https://github.com/user-attachments/assets/4b1d6ab2-f6f5-4a62-bd0b-046e9a27ab54" />

# Results and Impact
<ul>
  <li>Reduced reporting time by ~80%</li>
  <li>Eliminated manual data entry</li>
  <li>Enabled near real-time reporting</li>
  <li>Improved data accuracy and consistency</li>
  <li>Unique Identifiers assigned; helps overwrite data and avoid duplications</li>
  <li>Live visibility for stakeholders</li>
</ul>
