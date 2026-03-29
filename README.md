# Data Lakehouse on Databricks

A cloud-based data lakehouse built on **Databricks** and **Delta Lake**, following the **Medallion Architecture** (Bronze → Silver → Gold) to transform raw data into analytics-ready datasets.

---

## Architecture

```
Raw Data
   │
   ▼
┌─────────┐     ┌─────────┐     ┌────────┐
│  Bronze │ --> │  Silver │ --> │  Gold  │
│  (Raw)  │     │(Cleaned)│     │(Analytics-ready)│
└─────────┘     └─────────┘     └────────┘
```

| Layer  | Description |
|--------|-------------|
| **Bronze** | Raw data ingested as-is from source files. Schema enforced on read. |
| **Silver** | Cleaned, normalised, and deduplicated data. Business rules applied. |
| **Gold** | Aggregated, analytics-ready datasets optimised for reporting and querying. |

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| **Databricks** | Cloud compute & notebook environment |
| **Apache Spark / PySpark** | Distributed data transformation |
| **Delta Lake** | ACID transactions, schema enforcement, time travel |
| **Python** | Scripting and transformation logic |
| **Git** | Version control |

---

## Project Structure

```
databricks-data-lakehouse/
├── datasets/          # Raw source data files
├── scripts/           # PySpark transformation notebooks
│   ├── bronze/        # Ingestion layer notebooks
│   ├── silver/        # Cleaning & normalisation notebooks
│   └── gold/          # Aggregation & analytics notebooks
└── .gitignore
```

---

## Getting Started

### Prerequisites

- Databricks workspace (Community Edition works)
- Apache Spark 3.x
- Python 3.8+

### Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/huongngh/databricks-data-lakehouse.git
   ```

2. **Upload to Databricks**
   - Import the notebooks from `scripts/` into your Databricks workspace
   - Upload the source files from `datasets/` to DBFS (Databricks File System)

3. **Run in order**
   ```
   bronze/ → silver/ → gold/
   ```
   Execute each layer's notebook sequentially. Each notebook reads from the previous layer's Delta table.

---

## ✨ Key Features

- **ACID Transactions** — Delta Lake guarantees data consistency across all writes
- **Time Travel** — Roll back to any previous version of a table for auditing or error recovery
- **Schema Enforcement** — Invalid records are rejected at ingestion, keeping data clean
- **Incremental Processing** — Only new or changed records are processed at each run
- **Self-documenting** — Notebooks include inline comments explaining each transformation step

---

## Data Flow

```
Source Files (CSV/JSON)
        │
        ▼  [Bronze notebook]
Delta Table: bronze_table
        │
        ▼  [Silver notebook] — clean, normalise, deduplicate
Delta Table: silver_table
        │
        ▼  [Gold notebook] — aggregate, enrich
Delta Table: gold_table
        │
        ▼
Analytics / BI Tools
```

---

## Lessons Learned

- Delta Lake's **time travel** was invaluable for catching and rolling back bad writes during development
- Enforcing schema at the Bronze layer prevents silent data corruption from propagating downstream
- Keeping each layer's logic in a separate notebook makes debugging and testing significantly easier

---

## License

This project is open source and available under the [MIT License](LICENSE).
