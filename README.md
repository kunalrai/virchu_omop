# Data Pipeline Framework

## Overview
This repository contains Azure Data Factory (ADF) pipeline definitions for a data ingestion framework. The architecture employs a modular approach with master control pipelines and task-specific execution pipelines.

## Components

### Master Job Pipeline (`pl_job_master`)
- Controls overall job execution
- Initiates and monitors task execution
- Handles job auditing and error handling

### Task Launcher (`pl_taskaudit_launcher`)
- Starts task audit processes
- Determines task configuration type
- Routes to appropriate ingestion pipeline

### Standard Ingestion Pipeline (`pl_standard_ingest`)
- Main data ingestion workflow
- Extracts data from source systems
- Processes data through landing, raw, and curated layers
- Supports Databricks notebook integration

### SQL Server Ingestion (`pl_standard_ingest_sql_server`) 
- Specialized pipeline for SQL Server data sources
- Handles SQL query execution and data extraction
- Manages audit logging of execution metrics

## Data Flow
1. Job execution begins with `pl_job_master`
2. Tasks are retrieved and executed via `pl_taskaudit_launcher`
3. Each task uses `pl_standard_ingest` which determines source type
4. Source-specific pipelines (e.g., `pl_standard_ingest_sql_server`) extract data
5. Data is optionally processed through multiple data lake layers

## Environment Requirements
- Azure Data Factory
- Azure SQL Database (for ETL control)
- Azure Data Lake Storage
- Azure Databricks (optional)
- SQL Server Integration Runtime

## Usage
Configure tasks and jobs in the ETL control database tables, then trigger the job master pipeline with the desired job name parameter.

```
{
  "JobName": "your_job_name"
}
```

## Monitoring
The framework includes comprehensive auditing at both job and task levels, with logging of execution metrics and error handling.
