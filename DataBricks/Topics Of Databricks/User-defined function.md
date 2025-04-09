# 📚 Topic: UDF in Databricks

## 🔍 What is a UDF?
- A UDF (User-Defined Function) is a custom function that you write to do operations not already available in built-in PySpark/SQL functions.

You can use UDFs when your logic is too specific or not supported natively by Spark.

## 🧠 Is UDF in Python, PySpark, or SQL?
- ✅ You define UDF in Python (PySpark).

- ✅ You can also register it for SQL use.

- ❌ You can’t define UDFs purely in SQL — but you can use them in SQL after registering.

## 📘 Basic PySpark UDF Syntax:
```
from pyspark.sql.functions import udf
from pyspark.sql.types import StringType

def my_upper(s):
    return s.upper()

upper_udf = udf(my_upper, StringType())

df = df.withColumn("upper_name", upper_udf(df["name"]))
```
## 📘 Registering UDF for SQL:
```
spark.udf.register("myUpperSQL", my_upper, StringType())

df.createOrReplaceTempView("mytable")

spark.sql("SELECT myUpperSQL(name) AS upper_name FROM mytable").show()
```

## 🔄 Comparison Table: UDF vs SQL Function vs Python Function
|**Feature** |	UDF (in PySpark) |	SQL Function |	Normal Python Function |
|--------|--------------------|--------------|---------------|
|***Defined in***	|Python (PySpark) |	SQL |	Python |
|***Can be used in Spark SQL?***|	✅ Yes (if registered) |	✅ Yes |	❌ No |
|***Works on distributed data?*** |	✅ Yes (used on DataFrames) |	✅ Yes |	❌ No |
|***Native Spark performance?***|	❌ No (slower than built-ins) |	✅ Yes |	❌ Not used on big data |
|***Use case***|	Custom logic not in Spark |	Common data operations |	Any local task |
|***Needs schema definition?***|	✅ Yes (return type required) |	❌ No |	❌ No |

## ⚠️ When Should You Avoid UDFs?
UDFs are slower than built-in functions because:
- Spark can’t optimize them.

- They don’t support Catalyst Optimizer.

- Serialization overhead between JVM and Python (PySpark runs on JVM).

So, always prefer Spark built-in functions like `upper()`, `substring()`, `when()`, `regexp_replace()` before falling back to UDFs.
