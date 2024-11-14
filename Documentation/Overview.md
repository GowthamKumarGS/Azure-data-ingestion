# Project Overview
This project implements a dynamic data ingestion pipeline using Azure Data Factory (ADF) to copy multiple tables from an Azure SQL Database into Azure Data Lake Storage Gen2 (ADLS Gen2). The pipeline automates the selection of tables and their ingestion by dynamically configuring datasets, linked services, and file paths at runtime. This flexibility allows easy replication of the pipeline for various tables without individual configuration for each one.

## Objectives
- Dynamically ingest multiple tables from Azure SQL Database to ADLS Gen2.
- Automate pipeline configurations with parameters at various levels.
- Organize ingested data into dynamically structured directories in ADLS Gen2.

## Technologies Used
- **Azure Data Factory (ADF)**: For orchestrating the data flow and pipeline activities.
- **Azure SQL Database**: The source database containing multiple tables.
- **Azure Data Lake Storage Gen2 (ADLS Gen2)**: The destination for storing ingested data in a structured file format.

