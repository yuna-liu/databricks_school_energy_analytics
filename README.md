# School Energy Machine Learning – Data Engineering Foundation

## Overview

This repository contains the **data engineering foundation** of a school energy analytics project, implemented on **Databricks Lakehouse principles**.

The project focuses on **robust ingestion, correct parsing, and proper dimensional modeling** of large-scale energy time-series data, preparing it for downstream analytics and machine learning use cases such as forecasting and anomaly detection.

---

## Architecture Overview

**Landing Layer (Volume)**
- Raw TXT files (immutable)

**Bronze Layer (Logical)**
- energy_raw_lines  
  - file_name  
  - raw_line  
  - header_count  
  - ingestion_time  

**Silver Layer**
- energy_header (Dimension Table)
- energy_measurements (Fact Table)




The pipeline follows a **Bronze → Silver** separation with clear responsibilities and traceability.

---

## Implemented Components

### 00_configure.ipynb

- Project configuration
- Logical catalog and schema setup
- Shared parameters and conventions

---

### Raw Ingestion (Landing → Bronze)

Raw `.txt` files are stored unchanged in Databricks Volumes (Landing Layer).

Each file is ingested into a Bronze table at **line level**, preserving full traceability:

- One row per raw line
- File-level metadata (`file_name`)
- Per-file `header_count`
- Ingestion timestamp

Raw data is **never modified**.

---

### 01_silver_dm_and_fact_tables.ipynb
Silver Layer: Dimension & Fact Tables. Parsing is performed **table-driven**, without file-level for-loops.

#### `silver.energy_header` (Dimension Table)

- One row per building / file
- Building-level metadata extracted from file headers
- Examples:
  - location
  - year of construction
  - building identifiers

#### `silver.energy_measurements` (Fact Table)

- Time-series energy measurements
- One row per building per timestamp
- Numeric energy variables in wide format

Key design choices:

- Header and measurement separation based on `header_count`
- Correct file-level semantics preserved via `file_name`
- No dependence on file system ordering

---

## Design Principles

- Clear separation of raw and structured data
- Table-based transformations (not file-driven scripts)
- Dimension–Fact modeling
- ML-ready data structures
- Auditable and reproducible pipeline

---

## Environment

- Databricks (Community Edition)
- PySpark / Spark SQL
- Delta Lake concepts (logical)

> **Note:**  
> Physical catalog and schema placement may vary depending on Databricks edition.  
> This repository focuses on **pipeline design and data modeling**, not environment-specific deployment.

---

## Future Work: Machine Learning Roadmap

While this repository currently focuses on the data engineering layer, the implemented data model is intentionally designed to support downstream machine learning and advanced analytics.

Planned next steps include:

- **Time-series feature engineering**  
  Lag features, rolling statistics, and calendar-based features derived from Silver fact tables.

- **Load forecasting**  
  Per-building energy demand forecasting using statistical or regression-based models.

- **Anomaly detection**  
  Detection of abnormal consumption patterns using residual-based or statistical methods.

- **Experiment tracking**  
  Integration with MLflow for tracking parameters, metrics, and data versions.

The existing architecture ensures that these extensions can be added **without modifying the ingestion or parsing logic**, preserving data lineage and reproducibility.

---

## Summary

This project demonstrates a **production-oriented data engineering pipeline** that transforms raw energy data into structured, ML-ready datasets, emphasizing correctness, scalability, and future extensibility.


