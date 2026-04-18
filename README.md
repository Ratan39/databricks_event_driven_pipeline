# Incremental Order Data Pipeline with Databricks & Delta Lake

An automated ETL (Extract, Transform, Load) pipeline built on Databricks to handle incremental data ingestion from CSV sources into a managed Delta Lake environment.

## Project Overview
This project implements a robust data ingestion workflow using Spark and Delta Lake. It follows a modular notebook approach to process incoming order data, handle record updates (CDC/Upserts), and maintain a clean landing zone via automated file archiving.

## What is this project?

This project demonstrates an **event-driven pipeline** which is simple but efficient incremental data load pipeline using **Spark with Delta Lake** on **Databricks GCP**, where data is:

- Ingested from a source directory (CSV files)
- Written into a Delta staging table
- Merged into a Delta target table with logic to handle duplicates (based on tracking number)

## Architecture
The pipeline consists of two primary stages:

1.  **Stage Load (`order_stage_load`):** - Ingests raw CSV files from a Unity Catalog Volume.
    - Overwrites a staging Delta table to hold the current batch.
    - Moves processed files to an archive folder to ensure idempotency.
2.  **Target Load (`orders_target_load`):**
    - Reads the staged data.
    - Uses Delta Lake's `MERGE` capability to identify existing records via `tracking_num`.
    - Handles deduplication by removing old versions of records and appending the latest data.

## Why this project?

Data lakes can easily become messy and unreliable without structure and automation. This pipeline addresses:

- Automated staging of incoming files
- De-duplication of records during upsert
- Clean data lake management using **Delta Lake**
- Real-world event-driven patterns with merge logic

- ## Tech Stack
- **Platform:** Databricks
- **Storage:** Delta Lake (Unity Catalog Managed)
- **Engine:** Apache Spark (PySpark)
- **Language:** Python / SQL
- **Utilities:** Databricks Utilities (`dbutils`)

## Workflow Overview

1. Read incoming CSVs from a source folder
2. Write them to a staging Delta table (overwrite mode)
3. Merge new data into a target Delta table
   - If the record exists (same tracking number), delete old
   - Append the new data
4. Move processed files to archive

5. ## 📂 Notebook Details

### 1. Order Stage Load
- **Input:** `/Volumes/.../source/` (CSV)
- **Output:** `orders_stage` (Delta Table)
- **Tasks:** - Schema inference and data loading.
    - Atomic overwrite of the staging layer.
    - File system management (Moving files from Source to Archive).

### 2. Order Target Load
- **Input:** `orders_stage`
- **Output:** `orders_target` (Final Delta Table)
- **Logic:** - Checks for table existence.
    - Implements an Upsert strategy:
      - If `tracking_num` matches, the existing record is deleted.
      - New records from the stage are appended to the target.

## 📋 Prerequisites
- Databricks Workspace.
- Unity Catalog enabled (or DBFS configured).
- Source data in CSV format with a `tracking_num` column.
