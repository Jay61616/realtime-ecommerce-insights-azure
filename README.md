# 📦 Real-Time E-Commerce Order Intelligence and Sales Monitoring (Azure-Based)

> **Live Analytics Project** using Azure Event Hub, Databricks (PySpark), Azure Blob Storage, and Power BI – built entirely using U.S.-based simulated e-commerce data.

---

## 🔧 Tech Stack
- **Data Ingestion**: Azure Event Hub + Python Simulator  
- **Processing**: Azure Databricks (PySpark) – Bronze, Silver, Gold layers  
- **Storage**: Azure Blob Storage (Delta Format)  
- **Visualization**: Power BI Desktop (Live Connection to Gold Table)  
- **Version Control**: Git CLI & GitHub  

---

## 🚀 Project Overview

This real-time data engineering project simulates and processes live e-commerce order data to deliver insightful dashboards for U.S. retail operations. We built an end-to-end pipeline using Azure-native tools and open-source technologies, following a **Bronze → Silver → Gold** architecture.

<img width="6863" height="3358" alt="Strategy and planning" src="https://github.com/user-attachments/assets/e04b329f-9a6f-4f69-a199-54240d4c6fc7" />

---

## 🧪 Project Structure

```
real-time-ecommerce-insights-azure/
│
├── Databrick Notebooks/    # Bronze, Silver, Gold notebooks
│   └── 01_stream_orders_to_bronze.py
│   └── 02_cleaned_values_silver.py
│   └── 03_aggregated_to_gold.py
├── Power BI/               # Dashboard file (.pbix)
│   └── ecommerce_sales_dashboard.pbix
├── Simulator/              # Python script to simulate U.S. orders
│   └── generate_orders.py
└── README.md               # 📄 This file
```
---

## 🧱 Step 1: Data Simulation + Event Streaming

📦 Created a Python simulator to continuously send fake U.S.-based e-commerce orders (TX, NY, CA...) to **Azure Event Hub**. [generate_orders.py](simulator/generate_orders.py)

- Events included `order_id`, `state`, `city`, `product`, `quantity`, `price`, `timestamp`
- Used Python libraries: `uuid`, `random`, `json`, `time`, `azure-eventhub`
- Streamed events every second using a for-loop and `send_batch()` method

🪛 Commands:
- Used `pip install` to install Azure SDKs
- Set up Event Hub with namespace `e-commerce-namespace` and hub name `e-commerce-orders`
- Git-tracked simulator files with: `git add Simulator/`

---

## 🔁 Step 2: Real-Time Processing in Databricks

We created a **3-layer Delta Lake architecture** using **Structured Streaming** in PySpark.

### 🍂 Bronze Layer [01_stream_orders_to_bronze.py](databricks_notebooks/01_stream_orders_to_bronze.py)
- Read streaming data directly from Event Hub
- Parsed and flattened JSON events
- Wrote raw data to Azure Blob in Delta format

### 🔧 Silver Layer [02_cleaned_values_silver.py](databricks_notebooks/02_cleaned_values_silver.py)
- Cleaned, filtered, and type-casted Bronze data
- Removed nulls, bad records
- Stored refined data as a Delta table

### 🪙 Gold Layer [03_aggregated_to_gold.py](databricks_notebooks/03_aggregated_to_gold.py)
- Performed aggregation with **1-minute windows** (not hourly)
- Grouped by `state`, `product`, and `timestamp`
- Stored final result table (`gold`) to Blob in Delta

🧾 Notebooks:
- Named `1_bronze_stream.py`, `2_silver_clean.py`, `3_gold_agg.py`
- Git tracked using:  
```bash
git add Databrick\ Notebooks/
git commit -m "Add Bronze, Silver, Gold notebooks for streaming pipeline"
```

---

## 🗃️ Step 3: Azure Blob Storage – Delta Tables

Used Azure Blob Storage to persist all three layers in **Delta format**, organized as:

```
/mnt/data/bronze/  
/mnt/data/silver/  
/mnt/data/gold/
```

- Delta files were automatically updated via streaming write
- All Power BI visuals connected to the **Gold** layer

---

## 📊 Step 4: Power BI Dashboard (U.S. States)

Connected Power BI to **Databricks SQL Endpoint** to read the Gold Delta Table.

### 🔌 Connection Steps:
- Created SQL Warehouse in Databricks
- Connected Power BI using Azure Databricks connector (with token)
- Loaded `gold` table for visuals

### 📈 Visuals Created:
| Visual Type | Description |
|-------------|-------------|
| 📍 Map Chart | State-wise total sales |
| 📉 Line Chart | Sales trend over time (1-min window) |
| 🍩 Donut Chart 1 | Total sales per state |
| 🍩 Donut Chart 2 | Total items sold per state |
| 📄 Raw Data Table | Sorted by highest sales at top |


<img width="1418" height="792" alt="Screenshot 2025-08-07 135137" src="https://github.com/user-attachments/assets/1a9187b6-8ff9-4b77-81d2-43060aa981d1" />


🧾 Tracked Power BI with:
```bash
cd Power\ BI/
git add .
git commit -m "Add Power BI dashboard with state-wise visuals"
```

> ❌ Note: Forecasting was planned but **not implemented** in the final version.

---

## 🔗 Git Version Control

Used Git CLI for all version tracking:
- `git init`, `git remote add origin ...`
- `git add`, `git commit`, `git push` at every phase
- Maintained clear folder-by-folder commits

---

## ✅ Final Outcome

This project delivers a **fully functional real-time analytics pipeline** powered by Azure and Delta Lake. The dashboard provides operations teams with **minute-level insights** into U.S. e-commerce orders by product, state, and time.

---

## 📌 Next Steps (Optional)

- Add live forecasting using Power BI’s built-in analytics
- Schedule CI/CD automation for Databricks notebooks using GitHub Actions
- Extend simulator to include product categories or geolocation lat/long

---

## 📁 Folder Summary

```
📂 Simulator         → Sends real-time JSON orders to Event Hub
📂 Event Hub         → Azure setup: namespace + hub
📂 Databrick Notebooks → Bronze → Silver → Gold PySpark logic
📂 Storage           → Delta Lake directory structure
📂 Power BI          → Final .pbix with charts and map
📂 CICD              → Git commit history & tracking
```

---

**Author**: *Jaya Chandra Kadiveti*  
**GitHub**: [username](https://github.com/Jay61616)

---
