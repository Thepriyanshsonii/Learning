# 👀 What are Views in Databricks?
### 📌 Definition:
A View in Databricks is like a **virtual table.**
It **doesn’t store data**   , it just shows data from **other tables or queries.**

- You can use Views to organize, filter, or simplify complex queries — like giving a nickname to a big query.

## 🧩 Types of Views in Databricks
There are **3 main types** of Views in Databricks:

|**Type** |	**Scope** |	**Lifetime** |	**Best Use Case** |
|---------|-----------|--------------|--------------------|
| ***Temporary View*** |	Local to notebook |	Ends when notebook stops |	For quick, one-time use |
|***Global Temp View*** |	Across notebooks |	Ends when cluster stops |	Share between notebooks/sessions |
|***Standard (Permanent) View***|	In a database |	Stays forever (until dropped) |	For reusable logic|

## 1️⃣ Temporary View (CREATE OR REPLACE TEMP VIEW)
### 📌 What is it?

- Exists only in the current notebook or session.

- Deleted when you close the notebook or restart the cluster.

### ✅ Best For:
- Testing queries.

- Quick data previews.

**📦 Syntax:**
```
CREATE OR REPLACE TEMP VIEW my_temp_view AS
SELECT * FROM sales_data WHERE country = 'India';
```
📌 Use in same notebook:
`SELECT * FROM my_temp_view;`

## 2️⃣ Global Temporary View (CREATE OR REPLACE GLOBAL TEMP VIEW)

### 📌 What is it?
- Lives in a special `global_temp` database.

- Accessible across notebooks on the same cluster.

- Disappears when cluster stops.

### ✅ Best For:
- Sharing views between different notebooks or jobs.**

**📦 Syntax:**
```
CREATE OR REPLACE GLOBAL TEMP VIEW my_global_view AS
SELECT * FROM sales_data WHERE region = 'Asia';
```
📌 Use in any notebook:
`SELECT * FROM global_temp.my_global_view;` 

## 3️⃣ Permanent View (Just CREATE VIEW)

### 📌 What is it?
- Created inside a named database/schema.

- Stays forever (or until you delete it).

- Acts like a real table (but still doesn’t store data).

### ✅ Best For:
- Production dashboards, reports, long-term use.

**📦 Syntax:**
```
CREATE OR REPLACE VIEW analytics.top_customers AS
SELECT customer_id, SUM(total_spent) AS total
FROM sales_data
GROUP BY customer_id
ORDER BY total DESC;
```
📌 Use:
`SELECT * FROM analytics.top_customers;`

## ❗ Important Notes

- Views always reflect changes in the original table (live look).

- You cannot insert/update/delete data in views — they are read-only.

- Use SHOW TABLES or SHOW VIEWS to list views in your workspace.



