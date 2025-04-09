# 📘 Databricks: Mount and Unmount 
## 🔑 What is Mount in Databricks?
Mounting means connecting a storage location (like Azure Data Lake, AWS S3, etc.) to DBFS (Databricks File System), so you can access it like a folder.

### 🔓 Real-Life Example:
Imagine you rent a storage unit (like Azure Data Lake).
You keep your boxes (data) there.
Mounting is like creating a shortcut in your house (Databricks) to access that storage unit directly — so you don’t need to go outside every time.

### 🧠 Why Use Mounting?
- You don’t have to write credentials every time.

- Easy to read/write data using normal paths.

- Works like a local folder: /mnt/your-mount-name

### ✅ Syntax to Mount a Storage Location:
```
dbutils.fs.mount(
  source = "your-cloud-path",  
  mount_point = "/mnt/mymount",  
  extra_configs = {"key": "value"}  # Authentication credentials
)
```
### 🧹 Syntax to Unmount:
`dbutils.fs.unmount("/mnt/mymount")`
- This disconnects the mount from DBFS.

## 🚀 Real-Life Scenarios Where Mounting is Used:

### 🧊 1. Data in Azure Data Lake (ADLS)
**Example:**
- You store data in Azure container rawdata under storage account myaccount.

Mount command:
```
dbutils.fs.mount(
  source = "wasbs://rawdata@myaccount.blob.core.windows.net/",
  mount_point = "/mnt/raw",
  extra_configs = {"fs.azure.account.key.myaccount.blob.core.windows.net": "your_access_key"}
)
```
### 💡 Questions Wasbs vs abfss 
You’ve seen `wasbs://` and `abfss://` — both are used to connect to Azure storage, but they mean different things.

|**Protocol**|	**Stands For** |	**Used With** |**Supports Mount?**|	**Secure?**|
|--------|----------|--------|--------|-----|
|***wasbs://***|	Windows Azure Storage Blob over HTTPS |Azure Blob Storage (classic) |	✅ Yes |	🚫 Less secure |
| ***abfss://*** |	Azure Blob File System Secure |	Azure Data Lake Storage Gen2 |	❌ No  (use direct access) |	✅ More secure |

**🔍 What is `wasbs://?`**
- 📦 Used for Azure Blob Storage

- ✅ Can be used with dbutils.fs.mount()

- 👴 Legacy style (older than ADLS Gen2)

- 🔑 Needs storage account key for authentication

- ❌ Not optimized for big data and hierarchical file systems
**Example**
`wasbs://<container-name>@<account-name>.blob.core.windows.net/`

**🔐 What is abfss://?**
- 🚀 Used for Azure Data Lake Storage Gen2

- ❌ Cannot be used with dbutils.fs.mount() (Databricks says no mount with abfss)

- ✅ Best for secure, modern, and high-performance data access

- 🔐 Uses Azure Active Directory (AAD) + OAuth tokens (not storage key)

- 🧱 Supports hierarchical namespace (folders within folders)

**Example**
`abfss://<container-name>@<account-name>.dfs.core.windows.net/
`\

**🔄 So When to Use What?**
|**Situation**|	**Use This Protocol**|	**Why?**|
|-----------|-----------|-------------|
|Mounting with access key |	wasbs:// |	|Supports dbutils.fs.mount()|
|Direct read from ADLS Gen2 |	abfss:// |	 Secure, supports AAD + OAuth |
|You want fast, secure access |	abfss:// |	Best for production & big data |
| Legacy blob storage	| wasbs:// |	Works well with older accounts |
 
 ### 🧠 Tip for Memory:
- wasbs = "W" for Weak security (uses key)

- abfss = "A" for Azure AD + Advanced Security

### ✅ Pro Tip:
- If you’re working with **modern ADLS Gen2**, use abfss:// for direct access, not mounting.
- If you **must mount**, and you're okay with using a storage key, use wasbs://.

### 🌲 2. Data in AWS S3
Mounting S3 is less recommended due to security and performance concerns — use direct access (read below in scenarios).

But if needed, it works like this:
```
dbutils.fs.mount(
  source = "s3a://your-bucket-name",
  mount_point = "/mnt/mybucket",
  extra_configs = {"fs.s3a.access.key": "your_access_key", "fs.s3a.secret.key": "your_secret_key"}
)
```

### ☁️ 3. Accessing Data Without Mount (Direct Read)
Sometimes you don’t mount at all, and just read the data directly from its location (best for S3, API, etc.).
**Example**
`df = spark.read.json("s3a://my-bucket-name/data.json")`

**This is used when:**

- You don’t want to store credentials.
- You use temporary credentials (like tokens).
- You read from API responses or external services.

### 🛰️ 4. Data from API – No Mount Needed
If your data comes from an API:

- You call the API using requests or similar library.

- You convert the response to a DataFrame.

- Mounting doesn’t apply here.

```
import requests
import json

response = requests.get("https://api.example.com/data")
data = response.json()
df = spark.createDataFrame(data)
```
✅ No mount needed because you're getting data in memory, not from storage.

## 📁 When to Use Mount vs. Direct Access?

|Use Case |	Use Mount? |	Why? |
|---------|------------|---------|
|Azure Blob or Data Lake |	✅ Yes |	Works well with dbutils.fs.mount |
|AWS S3 (small usage) 	|❌ Prefer Direct |	Better for security, flexibility |
|API Data |	❌ No |	You pull data via HTTP, not files|
|Temporary Credential Use |	❌ No |	Mounting stores secrets longer |
|Frequent data access|	✅ Yes|	Speeds up file path usage|
|Rare or one-time access |	❌ No |	Mounting is unnecessary overhead|

## 🧪 Bonus: Check Mounted Folders  
`display(dbutils.fs.mounts())`

## 🧼 Clean Up: Unmount When Not Needed
`dbutils.fs.unmount("/mnt/raw")`

Why unmount?
- Save clutter

- Reset connections

- Rotate credentials

