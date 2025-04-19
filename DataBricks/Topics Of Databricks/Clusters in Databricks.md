# Clusters in Databricks 

### 💡 First: What is a cluster in Databricks?
A cluster is a group of computers working together to run your code, process data, and execute notebooks in Databricks.

It's like a powerful engine behind your data tasks.

## 🔥 Types of Clusters in Databricks (as of now)
**Databricks has mainly 3 types of clusters:**
### 1. Interactive Clusters (a.k.a All-Purpose Clusters)

**🧠 Simple Meaning:**
You use these when you’re exploring data or developing code in notebooks.

**⚙️ Bit-Level Details:**
- Stays alive when you're working.

- Used for manual, ad-hoc work.

- Multiple users can share it.

- Good for testing and debugging.

**🔍 Example:**
You open a notebook → write SELECT * FROM table → run the cell → it uses an interactive cluster to give you the result.

### 2. Job Clusters
**🧠 Simple Meaning:**
These clusters are for running production jobs — something scheduled or triggered, like a pipeline.

**⚙️ Bit-Level Details:**
- Created automatically when a job starts.

- Destroyed automatically when the job ends.

- Dedicated to only one job.

You can’t interact with it manually (no notebooks).

**🔍 Example:**
You set a job to run daily at 2 AM → Databricks creates a job cluster → runs the job → deletes the cluster.

### 3. Shared Clusters / Pool-Backed Clusters
**🧠 Simple Meaning:**
These are like reusable clusters that help you save time and cost.

**⚙️ Bit-Level Details:**
- Backed by a cluster pool — a pool is like a parking lot full of warm machines.

- Your cluster starts very fast using pre-warmed machines.

- Useful when you run many small jobs.

**🔍 Example:**
You have many small data tasks → instead of waiting for a new cluster every time, you use a shared cluster from the pool → faster start, less waiting.

## 🚦Quick Recap Table:

| cluster Type | Used For |	Starts Fast? |	Auto  Terminates? |	Used By |
|----------------|-----------|-------|------------|-----|
| Interactive |	Notebook exploration |	❌ (can be slow) |	❌ (unless set) |	Humans |
|Job|	Scheduled jobs, pipelines |	✅ |	✅	Code/jobs|
|Pool-backed(Shared)|	Many quick tasks |	✅ (very fast)|	Depends on usage|	Humans/Jobs |

**⚙️ Bit-Level Understanding:**
- Under the hood, clusters are Spark clusters (driver + workers).

- You define:

    - How many nodes (machines)

    - What kind of machines (CPU, memory)

    - What runtime (Spark version + extras)

- When you run a notebook, it’s sent to the driver, and the driver splits the work across the worker nodes.

- Job clusters are ephemeral: like disposable cups.
Interactive clusters are persistent: like reusable mugs