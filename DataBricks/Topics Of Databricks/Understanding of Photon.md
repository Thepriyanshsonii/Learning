# Photon in Databricks

### 💡 What is Photon in Databricks?
Photon is a next-generation query engine developed by Databricks, designed to make your queries run faster, especially on Delta Lake tables.

### 🧠 First: What is a "Query Engine"?
When you write SQL like:
```
SELECT * FROM sales WHERE amount > 1000
```
This doesn’t just magically return results. Behind the scenes, some engine must:

1.) Parse the query,
2.) Decide how to run it (called query planning),
3.) Read the data from storage,
4.) Filter and process it,
5.) Return results.

This process is what the query engine handles.

👉 Traditionally in Spark, this was handled by the Catalyst optimizer and Tungsten execution engine (both in JVM, the Java world).

### 🔥 So What’s Different with Photon?
Photon is a native vectorized query engine, written in C++ (not Java), which makes it lightning fast.

Let’s unpack this:

### 🧬 What Does "Native" Mean?
“Native” means it’s not running inside the Java Virtual Machine (JVM) like regular Spark. Instead, it’s written in C++, which talks directly to the CPU and memory.

👉 Native = Less overhead, more control = faster

### 🧪 What Does "Vectorized" Mean?
Instead of processing one value at a time, Photon processes multiple values at once using CPU-level vector instructions (like AVX-512).

### ⚙️ When is Photon Used in Databricks?
-> When you're using Databricks SQL (it's the default engine).
-> When you're using Delta Lake tables with optimized compute.
-> When you're running SQL or dataframe operations in       Databricks notebooks on DBR (Databricks Runtime) 9.1+ (and Photon is enabled).

-- Photon can replace parts of Spark SQL execution behind the scenes, making it seamless for you.

### 📊 Performance Example
Let’s say you run this:

```
SELECT customer_id, SUM(amount)
FROM sales
WHERE country = 'US'
GROUP BY customer_id
With Photon:
```
It will read the Parquet files using native C++ code

Apply filters using SIMD (vector instructions)

🔍 Result: Up to *10-20x faster* in many real-world workloads!

### 📁 Does Photon Change Your Code?
Nope. Your code stays the same. Photon is like a turbocharged engine under the hood — you just get the speed.

