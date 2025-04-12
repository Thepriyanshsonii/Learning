# 🔐 Authentication Methods in Azure for Databricks Access
Databricks often needs to access Azure Storage (like ADLS Gen2, Blob Storage). To do this securely, we need to authenticate. There are 3 main methods:

1. Account Access Key

2. SAS Token

3. Service Principal (SPN)

### 🔍 Why Databricks Needs to Access Azure Storage?
Because that's where the real data lives!

**Think of this analogy:**
- Databricks is your data kitchen (where you cook/analyze).

- Azure Storage is your fridge/pantry (where the ingredients/data are stored)

If you want to do any of the following:

- Load data for analysis

- Save processed data

- Access logs or files

- Train machine learning models

***👉 You must access Azure Storage from Databricks!***

### 🔐 So Why Do We Need Authentication?
Because Azure Storage is secure and private — it doesn't let anyone access it just like that.

You must prove your identity before accessing the data — just like showing a keycard or ID before entering a secure building.

That’s why Databricks needs authentication methods like:

1. Account Access Key

2. SAS Token

3. Service Principal

These methods prove to Azure that:

- *✅ “Hey, I’m Databricks, and I’ve been given permission to read/write data here.”*

### 🌐 Is This Only for Azure or Other Cloud Providers Too?
Nope — this concept is the same across all cloud providers:

|**Cloud Provider** |	**Storage Type**|	**Authentication Methods&**|
|-------------------|-------------------|-------------------|
|***Azure*** |	ADLS, Blob Storage |	SAS, Access Key, Service Principal (Azure AD) |
|***AWS***|	S3 |	Access Key + Secret, IAM Roles, Temporary Tokens |
|***Google Cloud***|	GCS (Cloud Storage)	| Service Accounts (OAuth2), Signed URLs |

## 1️⃣ Account Access Key (Think: Master Key)

### 📌 What is it?
- A secret key given by Azure when you create a storage account.

- Like the main password to your storage.

- Stored in Azure Portal under your Storage Account > Access keys.

**✅ Pros:**
- Easy to use for quick testing.

- Gives full access to everything in the storage.

**❌ Cons:**
- Too powerful. If someone misuses it, your whole storage is compromised.

- Not recommended for production.

**📦 Example Use in Databricks:**
```
spark.conf.set("fs.azure.account.key.<storage_account_name>.dfs.core.windows.net", "<your-access-key>")
```

## 2️⃣ SAS Token (Think: Temporary Visitor Pass)
### 📌 What is it?
- SAS = Shared Access Signature

- A token (URL parameter) that gives limited access to specific files or folders for a limited time.

**✅ Pros:**
- Very secure (you control what, when, and how someone can access).

- No need to share the account key.

**❌ Cons:**
- Can expire quickly if you don’t manage time correctly.

- Hard to manage if you’re dealing with many tokens.**

**📦 Example Use in Databricks:**
```
spark.conf.set(
  "fs.azure.account.auth.type.<storage_account_name>.dfs.core.windows.net", "SAS"
)
spark.conf.set(
  "fs.azure.sas.token.provider.type.<storage_account_name>.dfs.core.windows.net",
  "org.apache.hadoop.fs.azurebfs.sas.FixedSASTokenProvider"
)
spark.conf.set(
  "fs.azure.sas.fixed.token.<storage_account_name>.dfs.core.windows.net",
  "<your-sas-token>"
)
```

## 3️⃣ Service Principal (Think: Trusted Employee with ID Card)
### 📌 What is it?
- An Azure AD identity used by apps (like Databricks) to authenticate.

- It's like a "user account for apps" with its own ID & password.

***Components:***
1. Client ID (username)

2. Client Secret (password)
3. Tenant ID (Azure organization identifier)

**✅ Pros:**
- Very secure and scalable.

- You can give fine-grained access control via Azure RBAC (Role-Based Access Control).

- Used in real-world production systems.

**❌ Cons:**
- Slightly more setup effort compared to SAS or keys.

**📦 Example Use in Databricks (OAuth-based auth):**
```
configs = {
  "fs.azure.account.auth.type": "OAuth",
  "fs.azure.account.oauth.provider.type": "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
  "fs.azure.account.oauth2.client.id": "<client-id>",
  "fs.azure.account.oauth2.client.secret": "<client-secret>",
  "fs.azure.account.oauth2.client.endpoint": "https://login.microsoftonline.com/<tenant-id>/oauth2/token"
}

dbutils.fs.mount(
  source = "abfss://<container-name>@<storage-account-name>.dfs.core.windows.net/",
  mount_point = "/mnt/<mount-name>",
  extra_configs = configs)
```
## 💡 Pro Tips

1. For **one-time secure access** (e.g., sharing a file): use ***SAS Token***.

2. For **long-term automation** (e.g., connecting Databricks to storage): use a ***Service Principal.***

3. Avoid using ***Account Keys** in production. Rotate them if ever exposed.