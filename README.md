# 🏢 Walmart Cloud Migration & Data Lakehouse Project

This project simulates an enterprise-scale **cloud data migration** and **analytics lakehouse** implementation for a fictional Walmart retail environment. The goal was to migrate historical data from an on-premise MySQL source to a cloud-native Delta Lake architecture using the Azure ecosystem. Key outcomes included dynamic schema handling, dimensional modeling, automated orchestration, and business insight generation.

---

## 📌 Project Objectives

- Migrate de-normalized and normalized data from MySQL to Azure Data Lake Storage Gen2.
- Implement a robust **schema evolution detection mechanism** with alerting.
- Build a scalable, modular **ETL pipeline** using Azure Data Factory, Databricks, and Synapse.
- Create a **star-schema data model** and store it in Delta format (Silver Layer).
- Perform aggregations and analytics (Gold Layer) and visualize in Power BI.
- Schedule and orchestrate the pipeline using **ADF triggers** and **Logic Apps**.

---



## 🗺️ Architecture Overview

![Azure Data Lakehouse Architecture](https://github.com/rinithreddy14/Mysql-to-azure-DataMigration/blob/main/diagram-export-7-15-2025-4_30_25-PM.png?raw=true)

## 🧱 Tech Stack

| Layer | Technologies Used |
|-------|-------------------|
| Source | MySQL |
| Ingestion | Azure Data Factory (Self-Hosted Integration Runtime), Copy Activity |
| Processing | Azure Databricks (PySpark), Unity Catalog, Access Connectors |
| Schema Validation | Custom logic in Databricks + Logic App (email trigger) |
| Storage | Azure Data Lake Gen2 (Bronze, Silver, Gold Containers) |
| Serving | Azure Synapse Analytics (Serverless SQL Pools, External Tables) |
| Visualization | Power BI |
| Orchestration | Azure Data Factory Pipelines + Scheduled Trigger |
| Version Control | GitHub (linked with ADF & Databricks) |

---

## 🔁 Pipeline Overview

###  Bronze Layer
- **Ingestion from MySQL**:
  - Two modes:
    - `Method 1:` One de-normalized orders table.
    - `Method 2:` Fully normalized dimension and fact tables (customers, products, dates, orders).
  - Dynamic ADF pipeline with `Lookup + ForEach + If-Else` to route files based on table name.
  - Schema versioning and validation logic applied.

###  Silver Layer
- **Transformation with Databricks**:
  - Cleansing, deduplication, null handling, and standardization.
  - Schema evolution checked per file ingestion.
  - Created a star-schema data model (`fact_orders`, `dim_customers`, `dim_products`, `dim_dates`).
  - Stored in Delta format with partitioning.

###  Gold Layer
- **Aggregation and Reporting**:
  - Performed KPIs such as:
    - Total Revenue
    - Return Rates by Product Category
    - Monthly Trends
    - Customer Retention Metrics
  - Aggregated data written to Gold Layer tables.
  - Tables exposed via Synapse external tables.

###  Power BI Dashboard
- Connected to Synapse Serverless.
- Built interactive visuals:
  - Return rate by category
  - Seasonal trends
  - High-return product segments
  - Estimated cost of returns

---

## Schema Evolution & Alerting

- **Logic**: If schema changes are detected (column mismatch or data type change), the pipeline:
  - Logs the error
  - Triggers an Azure Logic App
  - Sends email alert to notify stakeholders
- Controlled via custom schema reference files and validation notebooks.

---

## 🧪 Sample Dataset Summary

| Metric | Value |
|--------|-------|
| Total Records | 233,209 |
| Columns | 18 |
| Missing Values | `customer_id`: 210, `product_id`: 251 |
| Return Status Categories | Cancelled, Returned |
| Data Types | Numeric, Categorical, UUID |

---

## 🧠 Key Insights Extracted

| Metric | Value |
|--------|-------|
|  Return Rate Range | 5% – 14% (monthly) |
| Highest Return Category | Books (11.64%) |
|  Retention Strategy | Based on repeat customers & return frequency |
| Business Risk | High return rates linked to seasonal promotions |
| cost of Returns | Estimated using proxy measures (shipping, quantity, status) |

---

## 📅 Scheduling

- Entire pipeline runs **daily at midnight (IST)** using ADF trigger.
- Logic App runs only on schema anomaly detection.

---


