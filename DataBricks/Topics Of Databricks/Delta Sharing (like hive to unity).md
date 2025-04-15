# 📘 Delta Sharing – Notes for Databricks

**Delta sharing is not just for Databricks---->Databricks ,that is one part of databricks**
Even if you have any other recipient you can share the data to any other recipient (apart from databricks)

**No need to have same subscription in both databricks**

### Doing from 1 workspace to another 
- ***workspace_1*** (Provider)
- ***workspace_2*** (Reciever) (Databricks workspace)

**Go to catalog and then Delta Sharing**
1.) Create new recipient 
- go to ***workspace_2*** and write 
        `select current_metastore()`
    provide you metastore id copy it 

paste it in unique identifier in ***workspace_1***

- **So recipient is created who to share data**

1. click on share data 
2. provide name 
3. create 
4. add table or notebook to share
5. select recipient who to share 
6. go to recipient
        1. Share Grant
7. go to workspace_2
    1. provider id click it 
    2. create catalog 
     
        
(so if you are trying to transfer the data from work1 to work2 you can transfer or you can give the access to the tables as well as the notebooks )
 
**if you are trying to share data from one workspace to another we have to make sure that the second workspace also have uc enabled** ***MANDATORY***

**Through command)**
## 📦 Delta Sharing: From One Workspace to Another

### ⚙️ Architecture Overview
**Workspace A (Provider)**
   |
   |  *Shares Delta Table via Delta Sharing*
   ↓
**Workspace B (Recipient)**
   |
   |  *Connects via Delta Sharing Profile URL*
   ↓
**Reads the shared table like it's their own!**

### ✅ Steps to Share a Delta Table from One Workspace to Another

**🔵 In Workspace A (Data Provider)**
1. Enable Delta Sharing
Databricks Admin → Workspace Settings
🔘 Turn on: Delta Sharing Enabled

2. Create a Share
    `CREATE SHARE sales_share;`
3. Add a Table to the Share
    `ALTER SHARE sales_share ADD TABLE my_db.monthly_sales;`
4. Create a Recipient
    `CREATE RECIPIENT workspace_b_recipient;`
    *This creates a profile file (JSON) that contains secure credentials and sharing URL.*
5. Export Recipient Credential File
    `DESCRIBE RECIPIENT workspace_b_recipient;`
    *👉 Download the credential JSON URL*
    *👉 Send it securely to Workspace B*

**🟢 In Workspace B (Data Receiver)**
1. Create a Delta Sharing Catalog
Use the credential JSON to register the shared data:
```
CREATE CATALOG shared_sales
USING SHARE `sales_workspace.sales_share`
OPTIONS (*
  profile_file = 'dbfs:/mnt/delta_sharing/recipient_profile.json'
);
```
*✅ This registers the shared data in your workspace*

2. Query the Shared Table
    `SELECT * FROM shared_sales.my_db.monthly_sales;`
*🎉 You are now reading live data from Workspace A!*

## 🧩 Behind the Scenes
- No data is copied – only read access is given.

- Security is maintained – via secure profile tokens.

- Live updates – when Workspace A updates the table, B sees latest data.

## ✅ Best Practices
|**Task**|**Best Practice**|
|--------|-----------------|
|🔐 Share securely |	Use tokenized recipient profile |
|🔄 Revoke access |	Just DROP RECIPIENT |
| 🧪 Versioning	 |Use VERSION AS OF in queries |
| 📂 Use dedicated catalog |	Organize all shared data under one catalog |

## ❓ If I work on a shared Delta table in Work2, will it reflect in Work1?

No.
If you're in ***Workspace_2*** and make changes, they won’t reflect in ***Workspace_1*** original Delta table.

### 📌 Why?
Because:

- Delta Sharing is a read-only mechanism for the recipient (Work2).

- Only the data provider (Work1) can make changes (INSERT/UPDATE/DELETE).

- Work2 can only read the latest version shared — it cannot write or modify the table

### 🧠 Summary:
|**Action**	| **Work1 (Provider)**|	**Work2 (Recipient)**|
|-----------|---------------------|----------------------|
|Can Read Table |	✅|✅|
|Can Write to Table	|✅ |	❌ (read-only)|
|Changes seen by others	 |✅ (if written) |	❌ |


**⚠️ So:**
- Changes made in Work1 → Visible in Work2 ✅

- Changes attempted in Work2 → Not allowed ❌

- No bidirectional sync

**If you want bi-directional editing, you’d need to:**

1. Grant both workspaces access to the same storage layer

2. Or build pipelines to write back separately

## ❓ Can I share a Delta Table from Hive Metastore to Unity Catalog using Delta Sharing?
**Yes**, it is possible — but with some important conditions and limitations.

### ✅ What’s Possible
**You can:**
- Share a Delta table from Hive Metastore in Workspace 1

- Recipient Workspace (with Unity Catalog) can access the shared data via Delta Sharing

*This is read-only, secure access — just like normal Delta Sharing.*

### ❌ What’s Not Possible (Important!)
- You cannot register or directly access Hive Metastore tables inside Unity Catalog

- Unity Catalog does not let you "import" Hive tables into Unity Catalog

- Delta Sharing doesn’t convert Hive tables into Unity Catalog tables — it just lets you read them


## ✅ How to Achieve It
**🔵 In Workspace 1 (Hive Metastore)**
1. Enable Delta Sharing

2. Create a Share:
    `CREATE SHARE hive_to_unity_share;`
3. Add Hive table:
    `ALTER SHARE hive_to_unity_share ADD TABLE default.hive_table1;`
4. Create a recipient:
    `CREATE RECIPIENT unity_recipient;`
5. Export recipient credentials (JSON)

**🟢 In Workspace 2 (Unity Catalog-enabled)**
1. Upload the credential JSON

2. Register the shared catalog using SQL or UI:
```
CREATE CATALOG shared_hive_catalog
USING SHARE `workspace1.hive_to_unity_share`
OPTIONS (
  profile_file = 'dbfs:/mnt/profile/unity_profile.json'
);
```
3. Query like:
`SELECT * FROM shared_hive_catalog.default.hive_table1;`

✅ You're now reading a Hive Metastore table in a Unity Catalog workspace — via Delta Sharing, not actual integration.

**🧠 Summary**
|**Question** |	**Answer** |
|-------------|------------|
| Can Hive tables be shared to Unity? |	✅ Yes (via Delta Sharing) |
| Can Unity "convert" Hive tables in? |	❌ No |
| Can Unity write to Hive-shared table?|	❌ No (read-only) |
| Is data duplicated? |	❌ No (live read) |




