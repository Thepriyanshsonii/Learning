# Differnece between USING PARQUET and USING DELTA

### 🤔 What’s the Difference Between USING PARQUET and USING DELTA?

Even though both PARQUET and DELTA tables store data in Parquet files, there's a huge functional difference between them.

### 📁 1. Storage Structure
| **Aspect** | **PARQUET Table** | **DELTA Table** |
|--------|---------------|-------------|
| ***File Format*** | Plain Parquet files | Parquet files + `_delta_log` transaction logs|
| ***Folder Structure*** | Just files | Files + hidden `_delta_log/` folder |

➡️ Delta is built on top of Parquet, adding transaction tracking and metadata management.

### 📋2. How They're Queried & Managed
- Parquet tables are like “just files” — Spark has to scan and figure things out every time.

- Delta tables know their metadata, schema, and file versions — leading to smarter query planning and better performance.

### 🧪 Real World case
```
CREATE EXTERNAL TABLE `${db_name}`.`DC_RAW_MATERIAL_DTLS`
USING PARQUET
LOCATION '${delta_location}/pdrm/DC_RAW_MATERIAL_DTLS/';
```
                        **VS**
```
CREATE EXTERNAL TABLE `${db_name}`.`DC_CHEMICAL_DTLS`
USING DELTA
LOCATION '${delta_location}/pdrm/DC_CHEMICAL_DTLS/';
```
***👉 What this means:***

- The first table is just reading raw Parquet files — no versioning, no schema enforcement, no transactions.

- The second one is reading from a Delta Table, meaning it has _delta_log, it can do updates, and you can do cool stuff like MERGE, DELETE, OPTIMIZE, VACUUM, etc.

## ⚠️ What If You Use USING DELTA on a Non-Delta Path?
**You’ll get an error like:**
```
org.apache.spark.sql.AnalysisException: The specified path is not a Delta table.

```
- Because Delta looks for the _delta_log folder, and if it’s not there, it’s not Delta.

## 💡 Pro Tip:
If you have raw Parquet data but want to use Delta features, you can easily convert it to Delta like this:
```
CONVERT TO DELTA parquet.`/mnt/path/to/parquet_folder`

```
It'll create _delta_log and turn it into a full Delta Table! 🧙

**OR**

### 🧪 Imagine Like This:
**📁 Parquet folder:**
```
/mnt/data/table1/
  ├── part-00000.parquet
  ├── part-00001.parquet
```
**📁 Delta folder:**
```
/mnt/data/table2/
  ├── part-00000.snappy.parquet
  └── _delta_log/
       ├── 000000.json
       └── 000001.json
```
### 💡 Now the rule:
- 🔥 When you say USING DELTA, Spark goes looking for the _delta_log/ folder.

If it’s missing, Spark says:
- "Wait bro, this ain't a Delta table. I'm not doing this."

### 🚫 What causes this error?
Let’s say you write:
```
CREATE EXTERNAL TABLE my_table
USING DELTA
LOCATION '/mnt/data/table1/';

```
But /mnt/data/table1/ is just a folder of Parquet files, with no _delta_log...

### ❌ Boom! You get:
```
org.apache.spark.sql.AnalysisException: The specified path is not a Delta table.

```
Because Spark’s like:
- **"You told me it's a Delta table… but there’s no Delta stuff here, no _delta_log. So, I can't trust this."**

### ✅ How to Fix It?
**✅ Option 1: Use USING PARQUET if it's plain Parquet**
```
CREATE EXTERNAL TABLE my_table
USING PARQUET
LOCATION '/mnt/data/table1/';
```
**✅ Option 2: Convert the Parquet folder to Delta before using**
```
df = spark.read.parquet('/mnt/data/table1/')
df.write.format('delta').save('/mnt/data/table1/')  # adds _delta_log
```
Now this folder has _delta_log, and you can safely do:
```
CREATE EXTERNAL TABLE my_table
USING DELTA
LOCATION '/mnt/data/table1/';
```

### 🧠 Visual Analogy
- **Parquet folder** = normal building with rooms

- **Delta table folder** = same building but with a security camera system installed (_delta_log)

- **Spark:** “If you say this is a secure building (Delta), but I don't see cameras, I won't trust it.”
