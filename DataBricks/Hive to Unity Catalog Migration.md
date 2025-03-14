# Hive to Unity Catalog Migration 
*Learn how to migrate notebook from Hive metastore to Unity Catalog*

## *Hive to Unity Catalog Migration*

### 1.) Use Databricks Migration Tools
> Databricks provides automatic and manual methods to migrate metadata from HMS(Hive meta Store) to Unity Catalog.

**Method 1: Use Databricks Migration Assistant**
Databricks offers a **Migration Assistant** to automate the migration of tables, schemas, and permissions.
  1. Install the migration tool:

command:-  

`*%pip install databricks-migration-tool*`

1. Run migration using a Databricks notebook:

       command:-

      * `from databricks_migration_tool import migrate_hive_metastore_to_uc`*

       `*migrate_hive_metastore_to_uc(
       hive_metastore_catalog="hive_metastore",
       unity_catalog_target="main_catalog"
       )*`

  

### Method 2: Manual Migration (SQL Approach)

         If you prefer a manual approach , use SQL to **recreate databases,  tables and permissions**

    **1. Create a Unity Catalog schema**:
        command:-

        `*CREATE SCHEMA main_catalog.new_schema;*`

    2. **Migrate Tables from Hive Metastore to Unity Catalog**:

        command:-

        `*CREATE TABLE main_catalog.new_schema.my_table
        AS SELECT * FROM hive_metastore.old_schema.my_table;*`

    3. **Migrate External Tables**:
         command:- 

         *`CREATE EXTERNAL TABLE main_catalog.new_schema.my_ext_table
         LOCATION 's3://your-bucket/path/'
         AS SELECT * FROM hive_metastore.old_schema.my_ext_table;`*

     4. **Transfer Permissions**:
          command:-

         * `GRANT SELECT, MODIFY`* 

          `*ON TABLE main_catalog.new_schema.my_table TO user@example.com;*`

     **5.  Verify and Validate Migration**

           After migration:

                   →  Run queries on **Unity Catalog** tables to ensure data consistency.
                   →  Check **metadata and permissions**.

                   →  Use **Table Lineage UI** to confirm the migration.
           command:-

           `*SELECT * FROM main_catalog.new_schema.my_table LIMIT 10;*`

## **Next Steps**

- Enable **data lineage** in Unity Catalog for governance.
- Use **Delta Sharing** to securely share data across workspaces.
- Explore **automated data classification** and **access policies**.