# Retail Sales Analytics on Databricks (ETL + Medallion Architecture)

This project demonstrates how to build a **scalable ETL pipeline** in **Azure Databricks** to analyze Walmart-style retail sales data.  
We implement the **Medallion architecture (Bronze → Silver → Gold)** using **Delta Lake**, enabling clean, structured, and query-ready data for BI/ML.

---

## 🚀 Project Setup

### 1. Azure & Databricks Environment
1. Log in to **Azure Portal**.
2. Create a **Resource Group** (e.g., `retail-rg`).
3. Search for **Azure Databricks**, create a **workspace**:
   - Region: same as resource group
   - Pricing tier: Standard or Premium
4. Once deployed, launch **Databricks workspace**.

### 2. Compute Cluster
1. In Databricks, go to **Compute** → **Create Cluster**.
2. Choose runtime: `Databricks Runtime 12.x (with Spark + Delta Lake)`.
3. Set autoscaling (min 2, max 8 workers).
4. Start the cluster.

### 3. Uploading Data
1. Navigate to **Data → Add Data → Upload Files**.
2. Upload the Kaggle datasets:
   - `sales data-set.csv`
   - `features data set.csv`
   - `stores data set.csv`
3. Save them in **DBFS** (Databricks File Store) for ingestion.

---

## 🔄 ETL Pipeline Explained

ETL = **Extract → Transform → Load**

- **Extract**: Pull raw CSVs into Spark DataFrames.  
- **Transform**: Clean, cast types, standardize dates, handle nulls.  
- **Load**: Store data into **Delta tables** for ACID transactions and time travel.

This pipeline ensures reliability, scalability, and reusability.

---

## 🏛 Medallion Architecture

We organize data into **three structured layers**:

1. **Bronze Layer (Raw Ingestion)**  
   - Stores **raw ingested data** as-is.  
   - Example: `bronze_sales`, `bronze_features`, `bronze_stores`.

2. **Silver Layer (Cleaned & Enriched)**  
   - Data is **cleaned, joined, standardized**.  
   - Example: `silver_sales` (with correct schema & formats).

3. **Gold Layer (Analytics-Ready)**  
   - Star schema with **fact & dimension tables** for BI/ML.  
   - Example:
     - `fact_sales`: sales with CPI, unemployment, temperature.
     - `dim_store`: store metadata.
     - `dim_dept`: department metadata.
     - `dim_date`: calendar attributes.

---

## 📊 Project Flow

1. **Ingest CSVs → Bronze**  
   ```python
   spark.read.csv("/dbfs/FileStore/...").write.format("delta").save("/bronze/sales")
   ```
2. **Transform → Silver**
- Cast date as DateType.

- Normalize column names.

- Join datasets.

3. **Model → Gold**

- Build star schema (Fact + Dimensions).

- Partition by year for performance.

- Optimize with Z-Ordering.

- Analytics Examples

## 📈 Outcomes

Fully functional Delta Lake pipeline on Azure Databricks

Analytics-ready Gold layer for BI tools

Demonstrates end-to-end cloud data engineering with scalable architecture

## 🛠 Tech Stack

Azure Databricks

Apache Spark (PySpark)

Delta Lake

Medallion Architecture

SQL for Analytics

## 📂 Repository Structure
```
├── notebooks/
│ ├── 1_smoke_test.ipynb
│ ├── 2_setup.ipynb
│ ├── 3_source_code.ipynb
├── data/ (sample csvs)
├── README.md
```
## 📌 Next Steps

📊 Build Power BI dashboard for visual analytics

🤖 Extend with ML models + MLflow for forecasting

🔐 Add data quality checks with Delta Live Tables
