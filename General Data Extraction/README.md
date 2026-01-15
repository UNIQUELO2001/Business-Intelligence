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
