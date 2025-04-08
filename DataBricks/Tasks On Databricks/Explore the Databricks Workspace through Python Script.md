# Explore the Databricks Workspace through Python Script
*TASK Done: - Find l2 Tables from l1 notebooks*

# Explore the Databricks notebook

We can explore the Databricks notebook through this code:

```python
import requests
import base64
import os

# Securely fetch credentials from environment variables
DATABRICKS_INSTANCE = "https://adb-767114318956228.8.azuredatabricks.net/"
#URL of your databricks

DATABRICKS_TOKEN = "dapif5e9e989125e51c311c9c96faeb533f3-3"
# You Can generate the Databricks Token from your account option in you databricks 

if not DATABRICKS_TOKEN:
    raise ValueError("Databricks token not found. Please set it as an environment variable.")

# Notebook path (adjust this)
NOTEBOOK_PATH = "/Workspace/Shared/P360/DML/L1_DP360/NB_BOA" #path to your notebook

# API Endpoint
API_ENDPOINT = f"{DATABRICKS_INSTANCE}/api/2.0/workspace/export"

# Parameters
params = {
    "path": NOTEBOOK_PATH,
    "format": "SOURCE"  # Other options: "SOURCE", "DBC"
}

# Headers
headers = {
    "Authorization": f"Bearer {DATABRICKS_TOKEN}"
}

try:
    # Send request to Databricks API
    response = requests.get(API_ENDPOINT, headers=headers, params=params)
    response.raise_for_status()  # Raise exception for HTTP errors

    # Decode the base64 content
    content = base64.b64decode(response.json()["content"]).decode("utf-8")
    
    # Print or save the notebook content
    print(content)  # Or save to file: open("notebook.html", "w").write(content)

except requests.exceptions.RequestException as e:
    print(f"Request failed: {e}")
```

## Explanation of the code: -

This script is written in **Python** and uses the `requests` library to fetch a notebook from **Databricks** via an API call. The notebook content is downloaded in **base64-encoded format**, which is then decoded to human-readable text.

---

### **1. Importing Required Libraries**

```python
python
CopyEdit
import requests
import base64
import os

```

### ✅ **Why do we need these?**

- `requests`: Used to **send API requests** to Databricks and fetch data.
- `base64`: Used to **decode the notebook's content**, since Databricks sends it in an encoded format.
- `os`: (Although not used effectively here) It's often used to read **environment variables** securely instead of hardcoding credentials.

**🔍 Real-world analogy:**

Think of `requests` as a **messenger** who goes to a library (Databricks), asks for a book (notebook), and brings it back.

The book is **encrypted** (base64), and we need `base64` to **decode** it into readable text.

---

### **2. Setting Up Credentials and API URL**

```python
python
CopyEdit
DATABRICKS_INSTANCE = "https://my_url"
DATABRICKS_TOKEN = "my_token"

```

- `DATABRICKS_INSTANCE`: This is the **Databricks workspace URL** (the address of the library).
- `DATABRICKS_TOKEN`: A **secret key** needed to access Databricks. It’s like an **ID card** that proves you have permission to access the notebook.

**⚠ Why do we check for the token?**

```python
python
CopyEdit
if not DATABRICKS_TOKEN:
    raise ValueError("Databricks token not found. Please set it as an environment variable.")

```

- This ensures that a valid token is present; otherwise, the program stops with an error.
- **Good practice**: Avoid hardcoding tokens directly in the script. Instead, store them securely using **environment variables**.

**🔍 Real-world analogy:**

Imagine you’re visiting a **restricted library**. If you forget your **ID card (DATABRICKS_TOKEN)**, the guard won’t let you in.

---

### **3. Defining the Notebook Path and API Endpoint**

```python
python
CopyEdit
NOTEBOOK_PATH = "/mypath"
API_ENDPOINT = f"{DATABRICKS_INSTANCE}/api/2.0/workspace/export"

```

- `NOTEBOOK_PATH`: The **location of the notebook** inside Databricks.
- `API_ENDPOINT`: The **URL where the API request will be sent**.

**🔍 Real-world analogy:**

You tell the librarian:

📖 *"I want the book stored in the **'Science'** section".*

Here, `NOTEBOOK_PATH` is like the **section** in the library.

---

### **4. Setting API Parameters**

```python
python
CopyEdit
params = {
    "path": NOTEBOOK_PATH,
    "format": "SOURCE"  # Other options: "SOURCE", "DBC"
}

```

- `path`: Specifies **which notebook** to fetch.
- `format`: Specifies **how** the notebook should be sent (in this case, as raw source code).
    - `"SOURCE"` → Gives the notebook as Python or Scala code.
    - `"DBC"` → A compressed Databricks notebook file.

**🔍 Real-world analogy:**

Imagine you are asking the librarian:

📖 *"Give me the book in **plain text format** instead of an encrypted version."*

---

### **5. Setting Up Request Headers**

```python
python
CopyEdit
headers = {
    "Authorization": f"Bearer {DATABRICKS_TOKEN}"
}

```

- This **adds the authentication token** in the API request.
- `"Authorization": f"Bearer {DATABRICKS_TOKEN}"` → Tells Databricks, **"I am an authorized user, allow me access!"**

**🔍 Real-world analogy:**

When you visit a **VIP club**, the security guard checks if you have a **VIP pass** before allowing entry.

Here, the `"Authorization"` header acts as your **VIP pass**.

---

### **6. Sending the API Request**

```python
python
CopyEdit
try:
    response = requests.get(API_ENDPOINT, headers=headers, params=params)
    response.raise_for_status()  # Raise exception for HTTP errors

```

- `requests.get(...)`: Sends an API request to **fetch** the notebook.
- `response.raise_for_status()`: Checks if the **request failed** (e.g., wrong token, wrong path, or no internet).

**🔍 Real-world analogy:**

If you send someone to fetch a book but they return empty-handed, they should at least **explain the error** (book missing, restricted access, etc.).

---

### **7. Decoding the Response**

```python
python
CopyEdit
content = base64.b64decode(response.json()["content"]).decode("utf-8")

```

- `response.json()["content"]`: Gets the **notebook content**, but it is encoded in base64.
- `base64.b64decode(...)`: **Decodes** the base64 content into **readable text**.
- `.decode("utf-8")`: Converts it into a proper string format.

**🔍 Real-world analogy:**

The librarian gives you a book, but the text is **encrypted**. You use a **decoder** to turn it into readable text.

---

### **8. Printing or Saving the Notebook**

```python
python
CopyEdit
print(content)  # Or save to file: open("notebook.html", "w").write(content)

```

- `print(content)`: Displays the **notebook code** in the terminal.
- `open("notebook.html", "w").write(content)`: Saves the notebook to a file.

**🔍 Real-world analogy:**

Once you get a **translated book**, you can **read it or write it down** for later.

---

### **9. Handling Errors**

```python
python
CopyEdit
except requests.exceptions.RequestException as e:
    print(f"Request failed: {e}")

```

- If **something goes wrong** (e.g., wrong URL, bad internet, missing token), the error message is printed.

**🔍 Real-world analogy:**

If the librarian **can’t find the book**, they tell you:

📖 *"Sorry, the book is not available."*

---

### **Final Flow of the Code**

1️⃣ Import necessary libraries.

2️⃣ Set the **Databricks URL** and **authentication token**.

3️⃣ Specify **which notebook** to fetch.

4️⃣ Prepare **API parameters** (notebook format).

5️⃣ Set up **request headers** with authentication.

6️⃣ Send the API request.

7️⃣ If the request is **successful**, decode the **notebook’s content**.

8️⃣ **Print or save** the notebook content.

9️⃣ If an **error** occurs, handle it gracefully.

---

### **Conclusion**

This script is a simple **Databricks API client** that fetches a notebook, decodes its content, and displays it.

Would you like me to modify the code to include **better security** (e.g., reading the token from environment variables securely)? 🚀

4o

# Filter and Perform operation on content of the notebook

Here I am performing an operation to find the l2 tables from l1 notebook script and performing an operation on it for the location of the table is correct or not 

```python
import requests
import base64
import re

# Securely fetch credentials from environment variables
DATABRICKS_INSTANCE = "https://adb-767114318956228.8.azuredatabricks.net/"
DATABRICKS_TOKEN = "dapif5e9e989125e51c311c9c96faeb533f3-3"

if not DATABRICKS_TOKEN:
    raise ValueError("Databricks token not found. Please set it as an environment variable.")

# Folder path (adjust this to the folder containing multiple notebooks)
FOLDER_PATH = "/Workspace/Shared/P360/DML/L1_DP360/"

# API Endpoint for listing notebooks in a folder
LIST_API_ENDPOINT = f"{DATABRICKS_INSTANCE}/api/2.0/workspace/list"

# Parameters for listing notebooks
params = {
    "path": FOLDER_PATH
}

# Headers for API request
headers = {
    "Authorization": f"Bearer {DATABRICKS_TOKEN}"
}

# Function to fetch content of a notebook
def get_notebook_content(notebook_path):
    API_ENDPOINT = f"{DATABRICKS_INSTANCE}/api/2.0/workspace/export"
    params = {"path": notebook_path, "format": "SOURCE"}
    response = requests.get(API_ENDPOINT, headers=headers, params=params)
    response.raise_for_status()  # Raise exception for HTTP errors
    content_base64 = response.json()["content"]
    content = base64.b64decode(content_base64).decode('utf-8', errors='ignore')
    return content

# Function to find L2 tables in the notebook content
def find_l2_tables_in_notebook(content):
    l2_tables = set()
    L2_TABLE_NAME_REGEX = r"\b[\w_]*l2[\w_]*\b"
    CREATE_TABLE_REGEX = r"create\s+or\s+replace\s+table\s+(\w+\.\w+)\s+using\s+delta\s+location\s+'[^']+/l2[^']*"
    DESCRIBE_TABLE_REGEX = r"%sql\s+describe\s+extended\s+(\w+\.\w+)"
    
    # Find tables directly referenced with "l2" in their name
    l2_tables.update(re.findall(L2_TABLE_NAME_REGEX, content))

    # Find tables created using the CREATE OR REPLACE TABLE statement with "l2" in the location
    create_tables = re.findall(CREATE_TABLE_REGEX, content)
    for table in create_tables:
        l2_tables.add(table)  # Just add the full schema.table as found

    # Find tables referenced in %sql describe extended commands
    describe_tables = re.findall(DESCRIBE_TABLE_REGEX, content)
    for table_name in describe_tables:
        if 'l2_dp360' in get_table_location(table_name):
            l2_tables.add(table_name)
    
    return l2_tables

# Simulate checking table location (replace with actual API in a real scenario)
def get_table_location(table_name):
    if 'l2' in table_name:  # Simulate finding location for table names containing 'l2'
        return '/mnt/staging/l2/dp360/'
    return '/mnt/staging/other_location/'

# Get the list of notebooks from the folder
try:
    response = requests.get(LIST_API_ENDPOINT, headers=headers, params=params)
    response.raise_for_status()
    notebooks = response.json().get('objects', [])
    
    for notebook in notebooks:
        if notebook['object_type'] == 'NOTEBOOK':
            notebook_path = notebook['path']
            print(f"Analyzing notebook: {notebook_path}")
            content = get_notebook_content(notebook_path)
            l2_tables_found = find_l2_tables_in_notebook(content)
            if l2_tables_found:
                print(f"L2 tables found in {notebook_path}:")
                for table in l2_tables_found:
                    print(table)
            else:
                print(f"No L2 tables found in {notebook_path}.")
except requests.exceptions.RequestException as e:
    print(f"Request failed: {e}")

```