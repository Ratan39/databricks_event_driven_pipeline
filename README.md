# databricks_event_driven_pipeline


## What is this project?

This project demonstrates an **event-driven pipeline** which is simple but efficient incremental data load pipeline using **Spark with Delta Lake** on **Databricks GCP**, where data is:

- Ingested from a source directory (CSV files)
- Written into a Delta staging table
- Merged into a Delta target table with logic to handle duplicates (based on tracking number)

## Why this project?

Data lakes can easily become messy and unreliable without structure and automation. This pipeline addresses:

- Automated staging of incoming files
- De-duplication of records during upsert
- Clean data lake management using **Delta Lake**
- Real-world event-driven patterns with merge logic

## Workflow Overview

1. Read incoming CSVs from a source folder
2. Write them to a staging Delta table (overwrite mode)
3. Merge new data into a target Delta table
   - If the record exists (same tracking number), delete old
   - Append the new data
4. Move processed files to archive
