# Creating Tables using Parquet Files

## Files are in the mount point so 

```
dbutils.widgets.text('db_name', 'pdrm_core')
dbutils.widgets.text('delta_location', '/mnt/pdrm_core/')`

DB_NAME = dbutils.widgets.get('db_name')
DELTA_LOCATION = dbutils.widgets.get('delta_location')

%sql
DROP TABLE IF EXISTS `${db_name}`.`formula_master`;
CREATE EXTERNAL TABLE `${db_name}`.`formula_master`
USING PARQUET
LOCATION '${delta_location}/pdrm/formula_master/';

## To check the tables under this db_name 
%sql 
SHOW TABLES IN `${db_name}`;
```


## Create the list of tables dynamically 
```
directory_path = "/mnt/pdrm_core/pdrm"
files = dbutils.fs.ls(directory_path)
db_name = "pdrm_core"

table_names = [file.name.rstrip("/") for file in files if file.isDir()]

for table_name in table_names:
    full_path = f"/mnt/pdrm_core/pdrm/{table_name}"
    
    sql_drop_table =f"""
     DROP TABLE IF EXISTS {db_name}.{table_name};
    """

    sql_create_table = f"""
    CREATE EXTERNAL TABLE {db_name}.{table_name}
    USING DELTA
    LOCATION '{full_path}';
    """
    
    spark.sql(sql_drop_table)
    spark.sql(sql_create_table)
    print(f"Generated SQL Query: {sql_create_table}")
    
    print(f"Table {db_name}.{table_name} created successfully at {full_path}")

```




## Two sql query won't execute 
### ⚠️Never try this a single variable can't hold two queries 
    sql_query = f"""
    DROP TABLE IF EXISTS `${db_name}`.`{table_name}`;

    CREATE EXTERNAL TABLE `${db_name}`.`{table_name}`
    USING DELTA
    LOCATION '${directory_path}/{table_name}/';

    """