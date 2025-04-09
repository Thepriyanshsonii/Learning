# Delta Transaction Log (_delta_log)

### The Delta Transaction Log
Everyone talks about Delta Lake. But few truly understand the magic behind it — the Delta Transaction Log (aka _delta_log folder).

***This folder is the brain of every Delta table.***

## Let's understand deeply:

### 🧠 First: What Is a Delta Table?
A Delta Table is:
   - A Parquet-based table
   - With ACID transactions
   - Version control
   - Time travel
   - Schema evolution

Cool right? But how does that even work? Parquet files can’t do that alone.

*👉 That’s where _delta_log comes in.*

### 📂 What Is _delta_log? 
Inside every Delta Table’s directory (in DBFS or any storage), there’s a hidden folder:
```
/path/to/delta_table/_delta_log/

```
This folder contains **CRC**  & **JSON** or **Parquet files**, and those files record every change ever made to the table.

-> It’s literally the transaction log of the table.
 
### 🧱 What's Inside _delta_log?
Files named like: `00000000000000000000.json`(Version 0), `00000000000000000001.json`(version 1), etc.

Each file represents a commit — like Git.

It records:

- Which files were added/removed

- What operation was performed (insert, delete, overwrite)

- Schema changes

- Metadata updates

**It looks like this (simplified):**
```
{
  "add": {
    "path": "part-00000.parquet",
    "size": 512,
    "partitionValues": {},
    "dataChange": true
  }
}
```
### 🛠️ Example: Time Travel
```
-- See data as it was 3 versions ago (Specifying version 3)
SELECT * FROM my_table VERSION AS OF 3;
```
➡️ Databricks reads version 3’s _delta_log JSON file to reconstruct the table exactly as it was at that point.