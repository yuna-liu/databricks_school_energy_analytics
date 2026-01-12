# Databricks School Energy Analytics (Data Engineering Foundation)

## Overview

This project implements a **data engineering foundation for school energy analytics**
using **Databricks Lakehouse principles**.

The goal of the project is to transform **semi-structured energy text files**
into **clean, analytics-ready tables**, following a layered architecture
that supports downstream analytics, BI, and future data science use cases.

The scope of this repository focuses on **data ingestion, modeling, and transformation**. The pipeline follows a **Bronze → Silver** separation with clear responsibilities and traceability.

---

## Architecture Overview

The project follows a simplified Lakehouse-style architecture:

```text
Raw Files (TXT)
   ↓
Volumes (Landing / Bronze)
   ↓
Silver Layer
   - energy_header        (metadata / dimensions)
   - energy_measurements  (time-series facts)
   ↓
Gold Layer
   - energy_features_long (analytics-ready feature table)

```

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

## Future Work
Potential next steps building on this foundation include:
- Advanced analytics and KPI development
- Data quality checks and validation rules
- Aggregated Gold tables for reporting
- Feature engineering for time-based analysis
- Machine learning use cases built on top of existing Gold tables
(e.g. forecasting or anomaly detection)

---

## Summary

This project demonstrates a **production-oriented data engineering pipeline** that transforms raw energy data into structured, ML-ready datasets, emphasizing correctness, scalability, and future extensibility.


