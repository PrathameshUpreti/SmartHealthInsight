# SmartHealth Insights — Healthcare Data Engineering Platform
 
An end-to-end healthcare data platform built on **Microsoft Fabric**, implementing a full **Medallion Architecture** (Bronze → Silver → Gold) across multi-source, intentionally messy healthcare data — designed to demonstrate production-style ingestion, transformation, data quality handling, and business-ready analytics.
 
> Built on a Microsoft Fabric trial workspace. Git integration for trial capacities is limited, so this repository documents the architecture, pipelines, and notebooks with screenshots and code exports rather than a live-synced workspace.
 
---
 
## Architecture
 
```
Source Systems → Bronze Lakehouse → Silver Lakehouse → Gold Lakehouse → Power BI
```
 
**Sources**
- **On-premises SQL Server** — `Patients` and `Admissions` datasets, ingested incrementally via Fabric Copy Jobs / Pipelines
- **Amazon S3** — `Medical Tests` CSV data, landed in an S3 landing folder and ingested via a Fabric pipeline; files are moved to a processed folder and removed from landing after successful ingestion
- **Real-time source** *(in progress)* — a streaming component is being added to demonstrate real-time healthcare data processing
---
 
## Medallion Layers
 
### 🥉 Bronze
- Dedicated Bronze Lakehouse
- Patients and Admissions ingested incrementally from on-prem SQL Server
- Medical Tests ingested from Amazon S3 via a Fabric pipeline
- Source data preserved with minimal transformation
### 🥈 Silver
- Separate Silver Lakehouse
- Dedicated PySpark notebooks per source: `Silver_Patient`, `Silver_Admission`, `Silver_Medicaltest`
- Data profiling, cleaning, validation, and standardization, including:
  - Null handling
  - Duplicate handling
  - Data type validation
  - Business-rule validation (invalid dates, invalid email/phone formats, inconsistent categorical values, invalid relationships)
- Problematic records flagged with data-quality / rejection reasons where applicable
- Output: clean, validated Silver Delta tables
### 🥇 Gold
Business-oriented analytical tables built for stakeholder reporting:
- **Patient Summary**
- **Admission Summary**
- **Medical Test Summary**
- **Patient Health Summary** — combines patient, admission, and medical test data for holistic healthcare analytics
Designed to support Power BI reporting on:
- Patient demographics and trends
- Admissions by department
- Average length of stay
- Admission trends over time
- Medical test volume
- Normal vs. abnormal test result rates
- Patient-level health summaries
---
 
## Orchestration
 
- Fabric pipelines orchestrate the full Bronze → Silver → Gold flow
- Explicit dependencies ensure Silver processing runs only after successful Bronze ingestion
- Separate ingestion patterns per source type:
  - **SQL Server** — incremental load pattern
  - **Amazon S3** — file-based landing/processed pattern
---
 
## Tech Stack
 
| Layer | Technology |
|---|---|
| Lakehouse & Storage | Microsoft Fabric Lakehouse, Delta Lake |
| Ingestion | Fabric Data Factory Pipelines, Copy Jobs |
| Transformation | PySpark Notebooks |
| Sources | On-premises SQL Server, Amazon S3 |
| Reporting | Power BI |
| Orchestration | Fabric Pipelines (dependency-based) |
 
---
 
## Status
 
| Component | Status |
|---|---|
| Bronze ingestion (SQL Server + S3) | ✅ Complete |
| Silver transformations & data quality | ✅ Complete |
| Gold business tables | ✅ Complete |
| Power BI reporting layer | ✅ Complete |
| Real-time streaming ingestion | 🚧 In progress |
 
---
 
## Notes
 
This project intentionally works with messy, imperfect data to simulate real-world healthcare data conditions — missing values, duplicate IDs, invalid relationships, invalid dates, and inconsistent formats are handled explicitly in the Silver layer rather than assumed away.
 
For a visual walkthrough of the workspace, pipelines, and notebooks, see the accompanying screenshots / LinkedIn post.
 
