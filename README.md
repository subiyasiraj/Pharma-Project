# 💊 Pharma Sales Pipeline — Databricks

A end-to-end data engineering pipeline built on **Databricks Community Edition** using **PySpark**, **SQL**, and **Delta Tables**. The project simulates a real-world pharma sales data workflow — from raw CSV ingestion through to regional reporting — orchestrated by a single master pipeline notebook.

---

## 📁 Folder Structure

```
databricks-pharma-project/
│
├── 00_setup/
│   └── Config                  # Global variables — file path, DB name, table names
│
├── 01_ingestion/
│   └── ingest_pharma_sales     # Read CSV from DBFS, validate schema, create temp view
│
├── 02_transformation/
│   └── transform_sales         # Cast types, clean nulls, apply filters, add derived columns
│
├── 03_reporting/
│   └── report_by_region        # Revenue by region, top drugs, top sales reps
│
├── utils/
│   └── Common_functions        # Shared helper functions — validate_schema, log_count, check_nulls
│
└── master_pipeline             # Orchestrates all stages end to end
```

---

## 🚀 How to Run the Pipeline

### Prerequisites
- Databricks workspace (Community Edition or above)
- A running cluster attached to your notebooks
- `pharma_sales.csv` uploaded to DBFS at `/FileStore/tables/pharma_sales.csv`

### Steps

**1. Upload the data**
- Databricks → Data → Add Data → Upload File → DBFS
- Upload `pharma_sales.csv`
- Confirm path: `/FileStore/tables/pharma_sales.csv`

**2. Update config if needed**
- Open `00_setup/Config`
- Verify `CSV_PATH` matches your upload path

**3. Run the full pipeline**
- Open `master_pipeline`
- Set your widgets at the top:
  - `region` — filter by North / South / East / West or leave as `All`
  - `start_date` — e.g. `2024-01-01`
  - `end_date` — e.g. `2024-03-31`
  - `category` — drug category filter or `All`
- Click **Run All**

The master pipeline will automatically trigger all three stages in order and print a summary on completion.

### Running individual notebooks
Each notebook can also be run standalone in this order:
```
00_setup/Config  →  utils/Common_functions  →  01_ingestion  →  02_transformation  →  03_reporting
```

---

## 🛠 Tech Stack

- **Databricks** — notebook environment and cluster compute
- **Apache Spark (PySpark)** — distributed data processing
- **Delta Lake** — table format
- **SQL** — data exploration and reporting
- **Unity Catalog** — data governance (`pharma_catalog.sales`)
- **GitHub** — version control

---

## 📊 Dataset

`pharma_sales.csv` — 30 records of synthetic pharma sales data

| Column | Type | Description |
|---|---|---|
| sale_id | Integer | Primary key |
| sale_date | Date | Transaction date (Jan–Mar 2024) |
| drug_name | String | 5 drugs — Amoxicillin, Metformin, Atorvastatin, Paracetamol, Azithromycin |
| category | String | Antibiotic, Diabetes, Cardiac, Analgesic |
| region | String | North, South, East, West |
| rep_name | String | 4 sales reps |
| units_sold | Integer | Units per transaction |
| unit_price | Double | Price per unit |
| total_amount | Double | Revenue per transaction |
| expiry_date | Date | Batch expiry |
| batch_id | String | Batch reference |
