# 📘 Topic: Tables in Databricks 
## 📌 Why Tables?
In Databricks, **a table** is like a container that **stores structured data** in rows and columns 
    — just like a table in SQL.
    
-  You use tables to store, query, and analyze data using SQL or PySpark.

## ✅ Types of Tables in Databricks

| **Table Type**       | **Stored As** | **Managed by**     | **Persisted in DB?** | **Example Use Case**                             |
|----------------------|---------------|---------------------|-----------------------|--------------------------------------------------|
| ***Managed Table***   | All (As you Declare)        | Databricks          | Yes                   | Internal data pipelines                          |
| ***Unmanaged Table*** | All  (As you Declare)       | You                 | Yes                   | Data from external sources (ADLS)                |
| ***Delta Table***     | Delta         | You/Databricks      | Yes                   | Supports versioning, updates, deletes            |
| ***Temporary View***  | No            | Memory only         | No                    | One-time queries |

## 1️⃣ Managed Table

### 🔹 What is it?
- Databricks stores the data and metadata.

- Table data is saved in Databricks-managed location.

- If you drop the table, data is also deleted.

### 🧠 Analogy:
- Like storing files in your C drive – system managed.

**📦 Create Example (SQL):**
```
CREATE TABLE student_scores (
  id INT,
  name STRING,
  marks INT
);
```
This creates a managed table in your default DB (usually default).

## 2️⃣ Unmanaged Table (External Table)
### What is it?
- You provide the location (e.g., ADLS or mounted folder).

- Only metadata is managed by Databricks.

- Data is not deleted if you drop the table.

### 🧠 Analogy:
- Like connecting to a USB drive — system sees it, but doesn’t own it.

**📦 Example (SQL):**
```
CREATE TABLE employee_details (
  emp_id INT,
  emp_name STRING
)
USING DELTA
LOCATION '/mnt/raw_data/employee/';
```

## 3️⃣ Delta Table
### 🔹 What is it?
- A special table format in Databricks that supports:

- ACID Transactions

- Time Travel

- Updates & Deletes

- Efficient storage

***It can be managed or unmanaged.**

- **🔥 It is the default format used in modern Databricks systems.**

## 🤔 Which Format Should You Use?
| **Format** |	**Use When**|
|------------|--------------|
|***Delta*** |	You want updates, deletes, time travel, merge (UPSERT) |
|***Parquet***|	You need fast reads and smaller file sizes|
|***CSV***|	You’re ingesting raw data from external systems|
|***JSON***|	You deal with nested or semi-structured data|
|***Avro***|	You’re working in streaming/data exchange scenarios|
|***ORC***|	Your tools (like Hive/Presto) work better with ORC|

## 🔙 Delta Time Travel (Built-in Magic ✨)
### 📌 What is it?
- You can query old versions of a Delta table.

- Think of it like Undo or View Past Snapshot.
```
SELECT * FROM student_scores VERSION AS OF 2; 
or 
SELECT * FROM student_scores@v2;
```
***or***
```
SELECT * FROM student_scores TIMESTAMP AS OF '2025-04-07T12:00:00';
```
## ⚙️ Creating Tables — SQL vs PySpark: Which is Better?

|**Method**|	**When to Use**| 	**Pros**|
|----------|-------------------|------------|
|***SQL***|	Analysts / Straightforward table creation|	Simple, readable|
|***PySpark***|	When working with dynamic data, JSON, APIs|	Full control, dynamic handling|