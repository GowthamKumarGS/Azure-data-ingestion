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


### Parameterized Expressions and Their Purpose

1. **Dynamic SQL Query for Copy Activity**:
   - **Expression**: `@concat('SELECT * FROM ', item().schemaName, '.', item().tableName)`
   - **Explanation**: This query retrieves all data from each table dynamically, based on the schema and table name provided by the ForEach Activity in each iteration.

2. **Dynamic Items for ForEach Activity**:
   - **Expression**: `@activity('Get a list of tables to be processed').output.value`
   - **Explanation**: This allows the ForEach loop to iterate over the list of tables fetched by the Lookup Activity.

3. **File Path in ADLS Gen2**:
   - **Expression**: `@concat('data/', item().schemaName, '/', item().tableName, '/', formatDateTime(utcnow(), 'yyyy-MM-dd'), '.txt')`
   - **Explanation**: This path structure enables organizing data by schema and table, with a timestamped folder, making data easily navigable in ADLS Gen2.

4. **Linked Service Parameter References**:
   - **Expressions**: `@pipeline().parameters.serverName` and `@pipeline().parameters.databaseName`
   - **Explanation**: These values are pulled from pipeline parameters, allowing the linked service to dynamically connect to different servers and databases at runtime.

---

Including these screenshots and parameter explanations will provide a clear, end-to-end understanding of your dynamic pipeline process. Let me know if you need any more specific details!
