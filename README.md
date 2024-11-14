# Dynamic Data Ingestion Pipeline from Azure SQL Database to Azure Data Lake Gen2

## Overview
This project demonstrates an automated, parameterized approach to data ingestion from an Azure SQL Database to Azure Data Lake Storage Gen2. Using Azure Data Factory (ADF), the pipeline copies data from multiple tables, dynamically configuring datasets, file paths, and linked services at runtime to allow flexible and reusable ingestion.

## Pipeline Steps
1. **Lookup Activity**: Retrieves a list of tables and schema names from Azure SQL Database.
2. **Dynamic Linked Service**: Uses parameters to dynamically connect to the specified database.
3. **ForEach Loop**: Iterates over the list of tables for ingestion.
4. **Copy Activity**: Copies each table's data to ADLS Gen2 in a dynamically structured folder.

## Files and Structure
- **Documentation/**: Contains detailed documentation of each pipeline step.
- **Code/**: JSON definition of the dynamic data ingestion pipeline.
