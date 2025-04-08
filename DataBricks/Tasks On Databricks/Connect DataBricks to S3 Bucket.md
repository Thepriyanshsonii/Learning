# Connect DataBricks to S3 Bucket 
*There are two ways to connect S3 To databricks we will discuss here*

# Connection to Databricks To AWS S3

As we know we have two methods to establish the connection between databricks and AWS S3 bucket 
                
                 1.) Through IAM Role 
                 2.) Using ARN(Amazon Resource Name)

let’s discuss it in depth 

### Method 1: Using AWS Access Keys

This method involves using AWS Access Keys to authenticate and access the S3 bucket from Databricks.

**Step 1: Generate AWS Access Keys**

1. **Log in to the AWS Management Console:**
    - Navigate to the [AWS Management Console](https://aws.amazon.com/console/).
2. **Access the IAM Service:**
    - In the console, click on "Services" and select "IAM" under "Security, Identity, & Compliance."
3. **Create a New IAM User:**
    - In the IAM dashboard, click on "Users" in the sidebar, then click "Add user."
    - Enter a username (e.g., `databricks-s3-access`).
    - Select the "Programmatic access" checkbox to provide access via the AWS CLI, SDKs, or APIs.
4. **Set Permissions:**
    - Choose "Attach existing policies directly."
    - Search for and select the `AmazonS3FullAccess` policy to grant full access to S3. Alternatively, create a custom policy to restrict access to specific buckets or actions.
5. **Complete the User Creation:**
    - Review the settings and click "Create user."
    - Note down the **Access Key ID** and **Secret Access Key** provided at the end of this process. These credentials are essential for configuring access in Databricks.

**Step 2: Configure Databricks to Use AWS Access Keys**

1. **Open Your Databricks Workspace:**
    - Log in to your Databricks workspace.
2. **Navigate to the Cluster Configuration:**
    - Click on "Clusters" in the sidebar.
    - Select an existing cluster or create a new one by clicking "Create Cluster."
3. **Set Spark Configurations:**
    - In the cluster configuration page, scroll down to the "Spark Config" section.
    - Add the following configurations, replacing `<Your-Access-Key-ID>` and `<Your-Secret-Access-Key>` with the credentials obtained earlier:
4. **Install Necessary Libraries:**
    - In the "Libraries" tab of the cluster, click "Install New."
    - Select "Maven" and enter the following coordinates to add the AWS SDK and Hadoop AWS library:
        
        ```
        makefile
        CopyEdit
        com.amazonaws:aws-java-sdk:1.12.100
        org.apache.hadoop:hadoop-aws:3.3.1
        
        ```
        
5. **Restart the Cluster:**
    - After applying the configurations and installing the libraries, restart the cluster to ensure all settings take effect.
        
        ```
        vbnet
        CopyEdit
        spark.hadoop.fs.s3a.access.key <Your-Access-Key-ID>
        spark.hadoop.fs.s3a.secret.key <Your-Secret-Access-Key>
        
        ```
        

**Step 3: Access the S3 Bucket from Databricks**

1. **Mount the S3 Bucket:**
    - In a Databricks notebook, use the following command to mount the S3 bucket to Databricks File System (DBFS):
        
        ```python
        python
        CopyEdit
        dbutils.fs.mount(
          source = "s3a://<Your-Bucket-Name>",
          mount_point = "/mnt/<Mount-Name>"
        )
        
        ```
        
        Replace `<Your-Bucket-Name>` with the name of your S3 bucket and `<Mount-Name>` with your desired mount point name.
        
2. **Verify the Mount:**
    - List the contents of the mounted directory to verify successful access:
        
        ```python
        python
        CopyEdit
        display(dbutils.fs.ls("/mnt/<Mount-Name>"))
        
        ```
        

**Considerations:**

- **Security:** Embedding AWS Access Keys in configurations can pose security risks. Ensure that access keys are managed securely and rotated regularly.
- **Permissions:** Assign the least privilege necessary to the IAM user to adhere to security best practices.

### **Method 2: Using IAM Roles and Instance Profiles**

This method leverages IAM roles and instance profiles to grant Databricks access to S3 buckets securely without embedding AWS credentials.

**Step 1: Create an IAM Role with S3 Access**

1. **Log in to the AWS Management Console:**
    - Navigate to the [AWS Management Console](https://aws.amazon.com/console/).
2. **Access the IAM Service:**
    - In the console, click on "Services" and select "IAM" under "Security, Identity, & Compliance."
3. **Create a New Role:**
    - In the IAM dashboard, click on "Roles" in the sidebar, then click "Create role."
4. **Select Trusted Entity:**
    - Choose "AWS service" as the trusted entity.
    - Select "EC2" as the use case, then click "Next."
5. **Attach Policies:**
    - Attach the `AmazonS3FullAccess` policy to grant full access to S3. For more controlled access, create and attach a custom policy that specifies allowed actions and resources.
6. **Add Tags (Optional):**
    - Add any tags for identification or management purposes, then click "Next."
7. **Review and Create Role:**
    - Review the role details, assign a role name (e.g., `databricks-s3-role`), and click "Create role."

**Step 2: Create an Instance Profile and Attach the IAM Role**

1. **Create an Instance Profile:**
    - In the AWS Management Console, navigate to the IAM service.
    - Click on "Instance profiles" in the sidebar, then click "Create instance profile."
    - Enter a name for the instance profile (e.g., `databricks-s3-profile`) and click "Create."
2. **Attach the IAM Role to the Instance Profile:**
    - After creating the instance profile, select it from the list.
    - Click "Add role to instance profile."
    - Select the IAM role you created earlier (`databricks-s3-role`) and click "Add."

**Step 3: Add the Instance Profile to Databricks**

1. **Obtain the Instance Profile ARN(Amazon Resource Name):**
    - In the IAM console, navigate to "Instance profiles" and select your instance profile.
    - Copy the **Instance Profile ARN(Amazon Resource Name)** (e.g., `ARN(Amazon Resource Name):aws:iam::123456789012:instance-profile/databricks-s3-profile`).
2. **Add the Instance Profile to Databricks:**
    - Log in to your Databricks workspace as an administrator.
    - Click on your username in the top right corner and select "Admin Console."
    - Navigate to the "Instance Profiles" tab.
    - Click "Add Instance Profile."
    - Paste the **Instance Profile ARN(Amazon Resource Name)** into the provided field.
    - Optionally, specify an **IAM role ARN(Amazon Resource Name)** if your instance profile's associated role name and instance profile name do not match.
    - Click "Add."

**Step 4: Configure Databricks Clusters to Use the Instance Profile**

1. **Assign the Instance Profile to a Cluster:**
    - In your Databricks workspace, click on "Clusters" in the sidebar.
    - Select an existing cluster or create a new one by clicking "Create Cluster."
    - In the cluster configuration page, scroll down to the "Advanced Options" section and expand it.
    - Under the "Instance Profile" dropdown, select the instance profile you added earlier (`databricks-s3-profile`).
2. **Restart the Cluster:**
    - After assigning the instance profile, restart the cluster to apply the changes.

**Step 5: Access the S3 Bucket from Databricks**

1. **Mount the S3 Bucket:**
    - In a Databricks notebook, use the following command to mount the S3 bucket to Databricks File System (DBFS):
        
        ```python
        python
        CopyEdit
        dbutils.fs.mount(
          source = "s3a://<Your-Bucket-Name>",
          mount_point = "/mnt/<Mount-Name>"
        )
        
        ```
        
        Replace `<Your-Bucket-Name>` with the name of your S3 bucket and `<Mount-Name>` with your desired mount point name.
        
2. **Verify the Mount:**
    - List the contents of the mounted directory to verify successful access:
        
        ```python
        python
        CopyEdit
        display(dbutils.fs.ls("/mnt/<Mount-Name>"))
        
        ```
        

**Considerations:**

- **Security:** Using IAM roles and instance profiles enhances security by avoiding the need to embed AWS credentials within Databricks configurations.
- **Permissions:** Ensure that the IAM role has the least privilege necessary to access the required S3 buckets, adhering to security best practices.