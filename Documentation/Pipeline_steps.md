# Pipeline Structure and Steps

1. **Lookup Activity: Retrieve Table List**
   - **Purpose**: Retrieve a list of tables and schema names from the source database.
   - **SQL Query**:
     ```sql
     SELECT QUOTENAME(t.name) AS tableName, QUOTENAME(SCHEMA_NAME(t.schema_id)) AS schemaName
     FROM sys.tables AS t;
     ```
     This query dynamically fetches table names and their schema, allowing processing of any table without hardcoding.

2. **Dynamic Linked Service Configuration**
   - **Parameters for Linked Service**: `serverName` and `databaseName` make the connection to the database dynamic.
   - **Why Parameters?**: Parameters enable runtime evaluation, allowing different databases or servers without modifying the linked service configuration.

3. **Parameterization at Multiple Levels**
   - **Pipeline Parameters**: Define default values for `serverName` and `databaseName` at the pipeline level, passing them to linked service and dataset parameters.
   - **Dataset Parameters**: Dataset parameters (`serverName`, `databaseName`, `schemaName`, and `tableName`) are populated by pipeline parameters and itemized in the ForEach activity.
   - **Data Flow**: Pipeline parameters flow into dataset and linked service parameters, ensuring each data transfer uses the correct dynamic source.

4. **ForEach Activity: Iteration Over Tables**
   - **Purpose**: The ForEach Activity iterates over the list of tables returned by the Lookup activity.
   - **Item Configuration**: The ForEach loop references the output of the Lookup activity, allowing each iteration to process a different table dynamically.

5. **Copy Activity: Table Data Transfer**
   - **Purpose**: Within each iteration, a Copy Activity transfers data from each table in Azure SQL Database to ADLS Gen2.
   - **SQL Query in Copy Activity**:
     ```sql
     SELECT * FROM [schemaName].[tableName]
     ```
     The query is parameterized to enable the Copy activity to retrieve data from the correct table and schema for each iteration.
   - **Destination in ADLS Gen2**: The file path is dynamically set based on the table and schema names, organizing data files effectively in the data lake.

6. **Dynamic File Path and Naming Configuration for ADLS Gen2**
   - **File Path Expression**: The final step of the pipeline involves setting a dynamic file path in ADLS Gen2, allowing each table to be saved in a unique, schema-based directory structure.
     ```json
     @concat(
         dataset().serverName, '/', 
         dataset().databaseName, '/', 
         dataset().tableName, '/', 
         'Year=', formatDateTime(utcnow(),'yyyy'), '/', 
         'Month=', formatDateTime(utcnow(),'MM'), '/', 
         'Day=', formatDateTime(utcnow(),'dd'), '/'
     )
     ```
   - **File Name Expression**: The output file name is dynamically generated as follows:
     ```json
     @concat(dataset().schemaName, '-', dataset().tableName, '.csv')
     ```

