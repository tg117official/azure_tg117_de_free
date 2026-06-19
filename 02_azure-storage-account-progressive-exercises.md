# Azure Storage Account Hands-on Exercises

## Lab Guide

**Module:** Azure Storage Account  
**Audience:** Beginners, data engineering learners, cloud beginners  
**Focus Areas:** Storage account creation, Blob Storage, containers, folders, uploads, SAS, access tiers, soft delete, versioning, lifecycle management, ADLS Gen2, Azure Files, Queue Storage, Table Storage, monitoring, cost review, and cleanup.

---

## Table of Contents

1. [How to Use This Lab Guide](#how-to-use-this-lab-guide)
2. [Learning Objectives](#learning-objectives)
3. [Beginner Explanation](#beginner-explanation)
4. [Prerequisites](#prerequisites)
5. [Required Datasets](#required-datasets)
6. [Naming Convention](#naming-convention)
7. [Cost and Safety Rules](#cost-and-safety-rules)
8. [Exercise Progression](#exercise-progression)
9. [Hands-on Exercises](#hands-on-exercises)
10. [Mini Project](#mini-project)
11. [Do's and Don'ts](#dos-and-donts)
12. [Common Mistakes and Troubleshooting](#common-mistakes-and-troubleshooting)
13. [Interview Questions](#interview-questions)
14. [Final Cleanup Checklist](#final-cleanup-checklist)
15. [References](#references)

---

# How to Use This Lab Guide

This document is designed for students who are learning Azure Storage Account from the beginning.

Complete the exercises in order. Do not jump directly to advanced exercises, because each exercise depends on concepts learned earlier.

Each exercise contains:

- Goal
- Beginner explanation
- Problem statement
- Required dataset
- Step-by-step instructions
- Expected output
- Validation
- Constraints
- Do's and don'ts
- Learning outcome

---

# Learning Objectives

By the end of this lab guide, you should be able to:

1. Create an Azure Storage Account.
2. Understand storage account, container, blob, and folder path.
3. Upload and download files using Azure Portal.
4. Organize files using folder-like paths.
5. Understand Hot, Cool, Cold, and Archive access tiers.
6. Generate secure temporary access using SAS.
7. Understand why storage data should remain private.
8. Enable soft delete and recover deleted files.
9. Enable blob versioning and test overwrite protection.
10. Create lifecycle management rules for cost optimization.
11. Understand Azure Data Lake Storage Gen2 and hierarchical namespace.
12. Create a medallion-style data lake layout.
13. Explore Azure Files, Queue Storage, and Table Storage.
14. Monitor storage account usage.
15. Clean up Azure resources safely.

---

# Explanation

## What Is an Azure Storage Account?

An Azure Storage Account is a cloud storage service in Microsoft Azure.

Think of it like a cloud-based storage box. Inside this storage account, you can store files, messages, tables, and shared file system data.

A storage account can contain different services:

| Service | Beginner Meaning | Common Use |
|---|---|---|
| Blob Storage | Stores files or objects | CSV, JSON, Parquet, images, logs |
| Azure Files | Cloud file share | Shared folders, network drive-like usage |
| Queue Storage | Stores messages | Application communication |
| Table Storage | NoSQL key-value storage | Simple structured data |

For data engineering, Blob Storage and Azure Data Lake Storage Gen2 are the most important.

---

## Storage Account vs Container vs Blob

| Term | Meaning |
|---|---|
| Storage Account | Main Azure storage resource |
| Container | Logical area inside Blob Storage |
| Blob | Actual file/object stored in a container |
| Folder Path | Folder-like organization for blobs |

Example:

```text
Storage Account: ststudentdemo001
Container: raw
Blob Path: sales/year=2026/month=06/day=19/sales.csv
Blob/File: sales.csv
```

---

## What Is a Container?

A container is like a top-level folder inside Blob Storage.

Example containers:

```text
raw
bronze
silver
gold
archive
temp
```

In data engineering, containers are often used to separate data zones.

---

## What Is a Blob?

A blob is the actual file stored inside a container.

Examples:

```text
customers.csv
orders.csv
products.json
sales.parquet
```

---

## What Are Access Tiers?

Access tiers help control storage cost based on how frequently data is used.

| Tier | Use Case | Beginner Rule |
|---|---|---|
| Hot | Frequently accessed data | Use for active practice files |
| Cool | Infrequently accessed data | Use for older but still readable data |
| Cold | Rarely accessed data | Use for rarely accessed data |
| Archive | Long-term archive | Do not use in beginner labs unless instructed |

Important: Archive data cannot be read immediately. It must be rehydrated first.

---

## What Is Redundancy?

Redundancy controls how Azure keeps extra copies of your data.

| Redundancy | Meaning | Student Recommendation |
|---|---|---|
| LRS | Copies data within one datacenter | Use for practice |
| ZRS | Copies data across zones in one region | Higher availability |
| GRS | Copies data to another region | Disaster recovery |
| GZRS | Zone + geo redundancy | Enterprise-grade |

For student labs, use **LRS** to control cost.

---

## What Is SAS?

SAS means Shared Access Signature.

It gives temporary, limited access to storage data.

Example:

You can give someone read access to `customers.csv` for 30 minutes without giving them your Azure password or storage account key.

---

## What Is Soft Delete?

Soft delete protects files from accidental deletion.

If a file is deleted, it can be restored during the retention period.

Example:

```text
Retention period: 7 days
```

---

## What Is Blob Versioning?

Blob versioning keeps previous versions of a file when it is overwritten.

Example:

If you upload `customers.csv` today and overwrite it tomorrow, Azure can preserve the old version.

---

## What Is Lifecycle Management?

Lifecycle management automates cost optimization.

Example rule:

```text
Move files older than 30 days from Hot to Cool.
Delete temp files older than 7 days.
```

---

# Prerequisites

Before starting, make sure you have:

| Requirement | Why It Is Needed |
|---|---|
| Active Azure account | To create resources |
| Azure Portal access | To perform hands-on steps |
| Subscription access | To create storage account |
| Budget alert | To avoid unexpected cost |
| Browser | Chrome, Edge, or Firefox recommended |
| Sample files | To upload and test storage |
| Basic file/folder knowledge | To understand paths |

---

# Required Datasets

Create the following files on your local machine before starting.

You can create them using Notepad, VS Code, or any text editor.

---

## Dataset 1: customers.csv

File name:

```text
customers.csv
```

Content:

```csv
customer_id,customer_name,city,email,created_date
1,Amit Sharma,Pune,amit@example.com,2026-06-01
2,Neha Patil,Mumbai,neha@example.com,2026-06-02
3,Rahul Verma,Nagpur,rahul@example.com,2026-06-03
4,Priya Deshmukh,Nashik,priya@example.com,2026-06-04
5,Karan Mehta,Delhi,karan@example.com,2026-06-05
```

---

## Dataset 2: orders.csv

File name:

```text
orders.csv
```

Content:

```csv
order_id,customer_id,order_date,amount,status
101,1,2026-06-10,2500,PLACED
102,2,2026-06-10,1800,PLACED
103,1,2026-06-11,3200,SHIPPED
104,3,2026-06-11,1500,CANCELLED
105,4,2026-06-12,7800,PLACED
```

---

## Dataset 3: products.json

File name:

```text
products.json
```

Content:

```json
[
  {"product_id": 1, "product_name": "Laptop", "category": "Electronics", "price": 60000},
  {"product_id": 2, "product_name": "Mouse", "category": "Accessories", "price": 800},
  {"product_id": 3, "product_name": "Keyboard", "category": "Accessories", "price": 1500},
  {"product_id": 4, "product_name": "Monitor", "category": "Electronics", "price": 12000}
]
```

---

## Dataset 4: sales_2026_06_19.csv

File name:

```text
sales_2026_06_19.csv
```

Content:

```csv
sale_id,product_id,customer_id,quantity,amount,sale_date
1,1,1,1,60000,2026-06-19
2,2,2,2,1600,2026-06-19
3,3,1,1,1500,2026-06-19
4,4,3,1,12000,2026-06-19
```

---

## Dataset 5: temp_test_file.csv

File name:

```text
temp_test_file.csv
```

Content:

```csv
id,message,created_at
1,this is a temporary file,2026-06-19
2,this file is used for lifecycle demo,2026-06-19
```

---

## Dataset 6: updated_customers.csv

This file is used for versioning exercise.

File name:

```text
updated_customers.csv
```

Content:

```csv
customer_id,customer_name,city,email,created_date
1,Amit Sharma,Pune,amit@example.com,2026-06-01
2,Neha Patil,Mumbai,neha@example.com,2026-06-02
3,Rahul Verma,Nagpur,rahul@example.com,2026-06-03
4,Priya Deshmukh,Nashik,priya@example.com,2026-06-04
5,Karan Mehta,Delhi,karan@example.com,2026-06-05
6,Sneha Kulkarni,Hyderabad,sneha@example.com,2026-06-19
```

---

# Naming Convention

Use these names during practice.

## Resource Group

```text
rg-storage-practice
```

## Regular Storage Account

Use a globally unique name:

```text
st<yourname>demo<number>
```

Example:

```text
stsandeepdemo001
```

## Data Lake Storage Account

```text
dl<yourname>demo<number>
```

Example:

```text
dlsandeepdemo001
```

## Containers

```text
raw
bronze
silver
gold
archive
temp
landing
```

---

# Cost and Safety Rules

Follow these rules carefully:

1. Use Standard performance.
2. Use LRS redundancy for practice.
3. Keep all containers private.
4. Do not upload large files.
5. Do not select Premium storage unless instructed.
6. Do not select GRS/GZRS unless instructed.
7. Do not share access keys.
8. Do not share SAS URLs publicly.
9. Keep SAS expiry short.
10. Delete practice resources after the session.
11. Check Cost Management after the lab.

---

# Exercise Progression

| Exercise | Topic | Level |
|---|---|---|
| 1 | Create Resource Group | Beginner |
| 2 | Create Storage Account | Beginner |
| 3 | Explore Storage Services | Beginner |
| 4 | Create Containers | Beginner |
| 5 | Upload Files | Beginner |
| 6 | Download Files | Beginner |
| 7 | Virtual Folders | Beginner to Intermediate |
| 8 | Blob Properties | Intermediate |
| 9 | Blob Metadata | Intermediate |
| 10 | Access Tiers | Intermediate |
| 11 | Blob SAS | Intermediate |
| 12 | Container SAS | Intermediate |
| 13 | Public Access Check | Intermediate |
| 14 | Soft Delete | Intermediate |
| 15 | Delete and Recover Blob | Intermediate |
| 16 | Blob Versioning | Intermediate |
| 17 | Overwrite and Check Versions | Intermediate |
| 18 | Lifecycle: Move Old Files to Cool | Intermediate to Advanced |
| 19 | Lifecycle: Delete Temp Files | Advanced |
| 20 | Access Keys | Intermediate |
| 21 | Azure Files | Intermediate |
| 22 | Queue Storage | Intermediate |
| 23 | Table Storage | Intermediate |
| 24 | Monitoring | Intermediate |
| 25 | ADLS Gen2 | Advanced |
| 26 | Medallion Containers | Advanced |
| 27 | ADLS Directory Structure | Advanced |
| 28 | RBAC Concepts | Advanced |
| 29 | Cost Review | Beginner |
| 30 | Cleanup | Beginner |

---

# Hands-on Exercises

---

# Exercise 1: Create a Resource Group for Storage Practice

## Level

Beginner

## Goal

Create one dedicated resource group for all storage account practice.

## Beginner Explanation

A resource group is like a folder in Azure. It helps you organize related resources.

If all practice resources are inside one resource group, cleanup becomes easy.

## Problem Statement

Create a resource group named `rg-storage-practice`.

## Required Dataset

No dataset required.

## Steps

1. Open Azure Portal.
2. Search for `Resource groups`.
3. Click **Resource groups**.
4. Click **Create**.
5. Select your subscription.
6. Enter resource group name:

```text
rg-storage-practice
```

7. Select region:

```text
Central India
```

8. Click **Review + create**.
9. Click **Create**.

## Expected Output

A resource group named `rg-storage-practice` should be created.

## Validation

Go to **Resource groups** and confirm that `rg-storage-practice` exists.

## Constraints

- Resource group name must be unique inside your subscription.
- Resource group region stores metadata. Resources inside it can still be created in other regions.

## Do's

- Use one resource group for this lab.
- Use a meaningful name.

## Don'ts

- Do not create lab resources in random resource groups.
- Do not use production resource groups for practice.

## Learning Outcome

You learned how resource groups help organize and clean up Azure resources.

---

# Exercise 2: Create a Basic Azure Storage Account

## Level

Beginner

## Goal

Create a storage account for Blob Storage practice.

## Beginner Explanation

A storage account is the main storage resource. Inside it, we create containers and upload files.

For student practice, we use Standard performance and LRS redundancy.

## Problem Statement

Create a storage account where you will upload customer, order, product, and sales files.

## Required Dataset

No dataset required.

## Steps

1. Open Azure Portal.
2. Search for `Storage accounts`.
3. Click **Create**.
4. Select your subscription.
5. Select resource group:

```text
rg-storage-practice
```

6. Enter a globally unique storage account name:

```text
st<yourname>demo001
```

Example:

```text
stsandeepdemo001
```

7. Select region:

```text
Central India
```

8. Performance:

```text
Standard
```

9. Redundancy:

```text
Locally-redundant storage (LRS)
```

10. Leave other options as default.
11. Click **Review**.
12. Click **Create**.
13. After deployment, click **Go to resource**.

## Expected Output

A storage account should be created successfully.

## Validation

On the storage account overview page, verify:

- Name
- Resource group
- Region
- Performance
- Redundancy

## Constraints

Storage account name:

- Must be globally unique.
- Must use lowercase letters and numbers only.
- Must be 3 to 24 characters.
- Cannot contain spaces, hyphens, or underscores.

## Do's

- Use Standard performance.
- Use LRS for practice.
- Use a clean naming convention.

## Don'ts

- Do not choose Premium for basic practice.
- Do not choose GRS/GZRS unless your trainer asks.
- Do not use sensitive personal information in the name.

## Learning Outcome

You learned how to create a cost-safe storage account.

---

# Exercise 3: Explore Services Inside the Storage Account

## Level

Beginner

## Goal

Identify the main services available inside the storage account.

## Beginner Explanation

One storage account can support multiple services. We will mainly use containers for Blob Storage, but it is important to know other services too.

## Problem Statement

Explore Containers, File shares, Queues, and Tables.

## Required Dataset

No dataset required.

## Steps

1. Open your storage account.
2. In the left menu, find the **Data storage** section.
3. Click:
   - **Containers**
   - **File shares**
   - **Queues**
   - **Tables**
4. Observe each page.
5. Do not create anything yet.

## Expected Output

You should be able to identify the four storage services.

## Validation

Answer these questions:

1. Where do we create Blob containers?
2. Where do we create file shares?
3. Where do we create queues?
4. Where do we create tables?

## Constraints

- Do not create resources in this exercise.
- Only observe the available services.

## Do's

- Read the menu names carefully.
- Understand that each storage service solves a different problem.

## Don'ts

- Do not assume every storage use case uses Blob Storage.
- Do not create public containers.

## Learning Outcome

You learned the major services available inside a storage account.

---

# Exercise 4: Create Blob Containers

## Level

Beginner

## Goal

Create containers to organize data.

## Beginner Explanation

A container is a logical area inside Blob Storage. For data engineering, we often divide data into zones such as raw, bronze, silver, gold, and archive.

## Problem Statement

Create containers for a small data engineering project.

## Required Dataset

No dataset required.

## Steps

1. Open your storage account.
2. Click **Containers**.
3. Click **+ Container**.
4. Create the following containers one by one:

```text
raw
bronze
silver
gold
archive
temp
```

5. For each container, set public access level as:

```text
Private
```

## Expected Output

The containers should be visible:

```text
raw
bronze
silver
gold
archive
temp
```

## Validation

Go to **Containers** and confirm that all six containers are listed.

## Constraints

Container names:

- Must be lowercase.
- Can contain letters, numbers, and hyphens.
- Cannot contain spaces.
- Should be meaningful.

## Do's

- Keep containers private.
- Use lowercase names.
- Use meaningful names.

## Don'ts

- Do not make containers public.
- Do not create unnecessary containers.
- Do not use unclear names like `abc`, `test1`, or `data123` in real projects.

## Learning Outcome

You learned how containers help organize data by purpose or layer.

---

# Exercise 5: Upload Files to the Raw Container

## Level

Beginner

## Goal

Upload sample CSV and JSON files.

## Beginner Explanation

In real projects, raw files usually land first in the raw zone. Later, they are processed and moved to bronze, silver, and gold layers.

## Problem Statement

Upload customer, order, and product files into the `raw` container.

## Required Dataset

Use:

```text
customers.csv
orders.csv
products.json
```

## Steps

1. Open your storage account.
2. Click **Containers**.
3. Open the `raw` container.
4. Click **Upload**.
5. Select `customers.csv`.
6. Click **Upload**.
7. Repeat the same for:
   - `orders.csv`
   - `products.json`

## Expected Output

Inside the `raw` container, you should see:

```text
customers.csv
orders.csv
products.json
```

## Validation

Click each file and verify:

- File name
- Size
- Last modified time

## Constraints

- Use small sample files.
- Do not upload private or sensitive data.
- Do not upload large media files in a student account.

## Do's

- Upload only the sample files.
- Check that each upload is successful.

## Don'ts

- Do not upload passwords, private documents, Aadhaar, PAN, or bank data.
- Do not upload large videos or backups for this lab.

## Learning Outcome

You learned how to upload files into Blob Storage.

---

# Exercise 6: Download and Validate a File

## Level

Beginner

## Goal

Download a blob and verify its content.

## Beginner Explanation

After uploading files, you should know how to retrieve and validate them.

## Problem Statement

Download `customers.csv` from the `raw` container and compare it with the original file.

## Required Dataset

Uploaded file:

```text
customers.csv
```

## Steps

1. Open the `raw` container.
2. Click `customers.csv`.
3. Click **Download**.
4. Open the downloaded file locally.
5. Compare it with your original file.

## Expected Output

The downloaded file should contain the same customer records.

## Validation

Confirm that these columns exist:

```text
customer_id, customer_name, city, email, created_date
```

## Constraints

- Browser may rename the file if a file with the same name already exists.
- Downloads usually go to the default Downloads folder.

## Do's

- Validate downloaded data.
- Keep local original files for comparison.

## Don'ts

- Do not assume upload succeeded without checking.
- Do not edit and re-upload unless instructed.

## Learning Outcome

You learned how to download and validate blobs.

---

# Exercise 7: Upload Data Using Virtual Folder Paths

## Level

Beginner to Intermediate

## Goal

Organize data using folder-like paths.

## Beginner Explanation

In data engineering, files are often organized by source, table, year, month, and day.

Example:

```text
sales/year=2026/month=06/day=19/sales_2026_06_19.csv
```

This helps tools like Spark process only required folders.

## Problem Statement

Upload sales data into a partition-style folder path inside the `raw` container.

## Required Dataset

Use:

```text
sales_2026_06_19.csv
```

## Steps

1. Open the `raw` container.
2. Click **Upload**.
3. Select `sales_2026_06_19.csv`.
4. Expand **Advanced** options.
5. In destination path/upload folder, enter:

```text
sales/year=2026/month=06/day=19
```

6. Click **Upload**.

## Expected Output

The file should be available at:

```text
raw/sales/year=2026/month=06/day=19/sales_2026_06_19.csv
```

## Validation

Navigate inside the `raw` container and confirm the file path.

## Constraints

- In normal Blob Storage, folders are virtual.
- If all blobs under a folder are deleted, the folder may disappear.

## Do's

- Use meaningful folder paths.
- Use `year=`, `month=`, and `day=` style for date partitioning.

## Don'ts

- Do not use spaces in folder names.
- Do not create random folder structures without business purpose.

## Learning Outcome

You learned how data engineers organize storage paths for analytics.

---

# Exercise 8: View Blob Properties

## Level

Intermediate

## Goal

View technical properties of a blob.

## Beginner Explanation

Every blob has properties such as URL, size, content type, last modified time, and access tier.

These details help with troubleshooting and validation.

## Problem Statement

View the properties of `customers.csv`.

## Required Dataset

Uploaded file:

```text
customers.csv
```

## Steps

1. Open the `raw` container.
2. Click `customers.csv`.
3. Open the **Properties** tab.
4. Observe:
   - Blob URL
   - Blob type
   - Size
   - Access tier
   - Last modified
   - Content type
   - ETag

## Expected Output

You should be able to view blob properties.

## Validation

Note down:

```text
Blob URL:
Size:
Access tier:
Last modified:
```

## Constraints

- Blob URL does not automatically mean public access.
- If the container is private, the plain URL cannot be opened without authorization.

## Do's

- Use properties to troubleshoot file uploads.
- Check last modified time after upload.

## Don'ts

- Do not share sensitive blob URLs publicly.
- Do not assume URL access means security is open.

## Learning Outcome

You learned how to inspect blob-level technical information.

---

# Exercise 9: Add Custom Metadata to a Blob

## Level

Intermediate

## Goal

Add metadata to a blob for tracking.

## Beginner Explanation

Metadata is extra information attached to a blob.

It does not change the file content.

Example:

```text
source_system = crm
file_type = customer_master
data_layer = raw
```

## Problem Statement

Add metadata to `customers.csv`.

## Required Dataset

Uploaded file:

```text
customers.csv
```

## Steps

1. Open the `raw` container.
2. Click `customers.csv`.
3. Open **Metadata**.
4. Add:

| Key | Value |
|---|---|
| source_system | crm |
| file_type | customer_master |
| uploaded_by | student |
| data_layer | raw |

5. Save metadata.

## Expected Output

Metadata should be saved on the blob.

## Validation

Reopen metadata and verify all key-value pairs.

## Constraints

- Do not store secrets in metadata.
- Metadata is not a replacement for a proper data catalog.

## Do's

- Use metadata for simple tracking.
- Keep metadata meaningful.

## Don'ts

- Do not store passwords, keys, or personal data in metadata.
- Do not depend only on metadata for governance.

## Learning Outcome

You learned how metadata helps describe files.

---

# Exercise 10: Change Blob Access Tier

## Level

Intermediate

## Goal

Understand and change access tier.

## Beginner Explanation

Access tiers control cost based on usage pattern.

Hot is for frequently used files. Cool is for less frequently used files. Archive is for long-term storage and cannot be read immediately.

## Problem Statement

Change `orders.csv` from Hot to Cool tier.

## Required Dataset

Uploaded file:

```text
orders.csv
```

## Steps

1. Open the `raw` container.
2. Click `orders.csv`.
3. Open **Properties**.
4. Find **Access tier**.
5. Change it to:

```text
Cool
```

6. Save changes.

## Expected Output

`orders.csv` should now show Cool access tier.

## Validation

Reopen properties and check the access tier.

## Constraints

- Cool tier is designed for data stored at least 30 days.
- Archive tier requires rehydration before data can be read.
- Tier changes can have cost implications.

## Do's

- Use Hot for active lab files.
- Use Cool only for demonstration.

## Don'ts

- Do not move active files to Archive during beginner practice.
- Do not randomly change tiers in production.

## Learning Outcome

You learned how access tiers support cost optimization.

---

# Exercise 11: Generate SAS URL for a Single Blob

## Level

Intermediate

## Goal

Create temporary read access for one file.

## Beginner Explanation

SAS allows controlled access without making the container public.

We will generate a read-only SAS URL for one file.

## Problem Statement

Generate a read-only SAS URL for `customers.csv` and test it in a private browser window.

## Required Dataset

Uploaded file:

```text
customers.csv
```

## Steps

1. Open the `raw` container.
2. Click `customers.csv`.
3. Click **Generate SAS**.
4. Select permission:

```text
Read
```

5. Set start time as current time.
6. Set expiry time to 30 minutes or 1 hour.
7. Generate SAS token and URL.
8. Copy **Blob SAS URL**.
9. Open an incognito/private browser window.
10. Paste the SAS URL and press Enter.

## Expected Output

The file should open or download.

## Validation

Confirm that the file is accessible using SAS URL even though the container is private.

## Constraints

- Anyone with the SAS URL can access the file until expiry.
- SAS works only during the valid time window.
- Permission should be minimum required.

## Do's

- Use only Read permission.
- Keep expiry short.
- Test in private browser.

## Don'ts

- Do not give Write/Delete permission unless required.
- Do not post SAS URLs on GitHub or public chats.
- Do not create long-expiry SAS for practice.

## Learning Outcome

You learned how to share one blob securely and temporarily.

---

# Exercise 12: Generate Container-Level SAS

## Level

Intermediate

## Goal

Create temporary access for multiple files in a container.

## Beginner Explanation

Blob SAS gives access to one file. Container SAS gives access to files in a container based on selected permissions.

## Problem Statement

Generate a read and list SAS for the `raw` container.

## Required Dataset

Files inside `raw` container:

```text
customers.csv
orders.csv
products.json
```

## Steps

1. Open your storage account.
2. Go to **Containers**.
3. Open or select the `raw` container.
4. Click **Generate SAS**.
5. Select permissions:
   - Read
   - List
6. Set expiry time to 1 hour.
7. Generate the SAS URL.
8. Open it in a private browser window.

## Expected Output

You should be able to access/list container contents based on selected permissions.

## Validation

Check whether files are visible or accessible using the SAS URL.

## Constraints

- Without List permission, users may not list container contents.
- More permissions mean more risk.
- Container SAS should be used carefully.

## Do's

- Use Read + List only for this lab.
- Keep expiry short.

## Don'ts

- Do not select Add, Create, Write, or Delete unless instructed.
- Do not share container SAS publicly.

## Learning Outcome

You learned the difference between blob SAS and container SAS.

---

# Exercise 13: Check Public Access Settings

## Level

Intermediate

## Goal

Confirm that data is not publicly exposed.

## Beginner Explanation

Private containers require authorization. Public containers may allow anonymous access.

For most data engineering projects, data must remain private.

## Problem Statement

Check storage account and container public access settings.

## Required Dataset

No dataset required.

## Steps

1. Open your storage account.
2. Go to **Configuration**.
3. Find:

```text
Allow Blob anonymous access
```

4. Keep it disabled unless trainer asks otherwise.
5. Go to **Containers**.
6. Open `raw`.
7. Confirm public access level is:

```text
Private
```

8. Try opening plain Blob URL without SAS in a private browser.

## Expected Output

The plain Blob URL should not allow access.

## Validation

If the plain URL fails without SAS, private access is working.

## Constraints

- Some tenants disable public access by policy.
- Public access is not recommended for private datasets.

## Do's

- Keep containers private.
- Use SAS or RBAC for controlled access.

## Don'ts

- Do not make business/customer/sales data public.
- Do not enable public access just for convenience.

## Learning Outcome

You learned why private access is safer.

---

# Exercise 14: Enable Blob Soft Delete

## Level

Intermediate

## Goal

Protect blobs from accidental deletion.

## Beginner Explanation

Soft delete works like a recycle bin for blobs.

If a blob is deleted, it can be recovered within the retention period.

## Problem Statement

Enable blob soft delete with 7-day retention.

## Required Dataset

No dataset required.

## Steps

1. Open your storage account.
2. Search for **Data protection**.
3. Enable **Soft delete for blobs**.
4. Set retention period:

```text
7 days
```

5. Save changes.

## Expected Output

Soft delete should be enabled.

## Validation

Reopen Data protection and confirm soft delete is enabled.

## Constraints

- Soft delete must be enabled before deletion.
- Deleted data retained by soft delete may still consume storage.
- Soft delete is not a full backup solution.

## Do's

- Enable soft delete for important data.
- Use a sensible retention period.

## Don'ts

- Do not assume deleted data is recoverable forever.
- Do not set very long retention blindly.

## Learning Outcome

You learned how soft delete protects against accidental deletion.

---

# Exercise 15: Delete and Recover a Blob

## Level

Intermediate

## Goal

Recover a deleted file using soft delete.

## Beginner Explanation

Now that soft delete is enabled, we will intentionally delete a sample file and restore it.

## Problem Statement

Delete `orders.csv` and recover it.

## Required Dataset

Uploaded file:

```text
orders.csv
```

## Steps

1. Open the `raw` container.
2. Select `orders.csv`.
3. Click **Delete**.
4. Confirm deletion.
5. Enable **Show deleted blobs**.
6. Locate deleted `orders.csv`.
7. Select it.
8. Click **Undelete**.

## Expected Output

`orders.csv` should be restored.

## Validation

Refresh the container and confirm `orders.csv` is visible again.

## Constraints

- Soft delete must have been enabled before deletion.
- Recovery works only within retention period.

## Do's

- Test with sample files only.
- Restore immediately during the lab.

## Don'ts

- Do not test deletion with important files.
- Do not assume recovery after retention expires.

## Learning Outcome

You learned practical blob recovery.

---

# Exercise 16: Enable Blob Versioning

## Level

Intermediate

## Goal

Protect files from accidental overwrite.

## Beginner Explanation

Soft delete protects from deletion. Versioning protects from overwrite.

If a file is overwritten, older versions can be preserved.

## Problem Statement

Enable blob versioning.

## Required Dataset

No dataset required.

## Steps

1. Open your storage account.
2. Go to **Data protection**.
3. Enable **Blob versioning**.
4. Save changes.

## Expected Output

Blob versioning should be enabled.

## Validation

Reopen Data protection and confirm versioning is enabled.

## Constraints

- Versioning can increase storage usage.
- Lifecycle rules may be needed to manage older versions.
- Some combinations of features may behave differently for hierarchical namespace-enabled accounts.

## Do's

- Use versioning for files that may be overwritten.
- Combine with lifecycle management in real projects.

## Don'ts

- Do not enable versioning without understanding retention and cost.
- Do not treat versioning as complete disaster recovery.

## Learning Outcome

You learned how versioning protects from accidental overwrite.

---

# Exercise 17: Overwrite a Blob and Check Versions

## Level

Intermediate

## Goal

Test versioning by overwriting a file.

## Beginner Explanation

To create a new version, upload a file using the same blob name.

If the file name is different, it will be treated as a different blob, not a version.

## Problem Statement

Overwrite `customers.csv` with updated data and check versions.

## Required Dataset

Use:

```text
updated_customers.csv
```

Before uploading, rename it locally to:

```text
customers.csv
```

## Steps

1. Rename `updated_customers.csv` to `customers.csv`.
2. Open the `raw` container.
3. Click **Upload**.
4. Select the updated `customers.csv`.
5. Choose overwrite if prompted.
6. Open `customers.csv`.
7. Look for version history or versions.

## Expected Output

You should see current and previous versions.

## Validation

Current file should have 6 customer records. Previous version should have 5 records.

## Constraints

- Versioning must be enabled before overwrite.
- Different file names do not create versions.

## Do's

- Use the same blob name for overwrite testing.
- Compare old and new file contents.

## Don'ts

- Do not confuse duplicate files with versions.
- Do not overwrite production files casually.

## Learning Outcome

You learned how Azure preserves previous versions of overwritten blobs.

---

# Exercise 18: Lifecycle Rule to Move Old Files to Cool Tier

## Level

Intermediate to Advanced

## Goal

Create a lifecycle rule for cost optimization.

## Beginner Explanation

Lifecycle rules automate storage management.

For example, old raw files can be moved from Hot to Cool tier automatically.

## Problem Statement

Create a rule to move raw files older than 30 days to Cool tier.

## Required Dataset

No new dataset required.

## Steps

1. Open your storage account.
2. Search for **Lifecycle management**.
3. Click **Add a rule**.
4. Rule name:

```text
move-old-raw-files-to-cool
```

5. Rule scope:

```text
Limit blobs with filters
```

6. Blob type:

```text
Block blobs
```

7. Condition:

```text
If base blobs were last modified more than 30 days ago
```

8. Action:

```text
Move to Cool storage
```

9. Add filter:
   - Container: `raw`

10. Save the rule.

## Expected Output

A lifecycle rule should be created.

## Validation

Open Lifecycle management and confirm the rule is visible.

## Constraints

- The rule may not run immediately.
- Files must satisfy the age condition.
- Lifecycle rules should be tested carefully.

## Do's

- Use filters.
- Start with safe movement rules before delete rules.

## Don'ts

- Do not apply broad delete rules.
- Do not move active files to Archive without understanding rehydration.

## Learning Outcome

You learned how lifecycle policies reduce storage cost.

---

# Exercise 19: Lifecycle Rule to Delete Old Temp Files

## Level

Advanced

## Goal

Automatically delete old temporary files.

## Beginner Explanation

Temporary files should not stay forever. Lifecycle rules can automatically delete old temp files.

## Problem Statement

Upload a temp file and create a rule to delete temp files older than 7 days.

## Required Dataset

Use:

```text
temp_test_file.csv
```

## Steps Part A: Upload Temp File

1. Open the `temp` container.
2. Upload `temp_test_file.csv`.

## Steps Part B: Create Lifecycle Rule

1. Open your storage account.
2. Go to **Lifecycle management**.
3. Click **Add a rule**.
4. Rule name:

```text
delete-old-temp-files
```

5. Rule scope:

```text
Limit blobs with filters
```

6. Blob type:

```text
Block blobs
```

7. Condition:

```text
If base blobs were last modified more than 7 days ago
```

8. Action:

```text
Delete blob
```

9. Filter:
   - Container: `temp`

10. Save the rule.

## Expected Output

A lifecycle rule should be created for the temp container.

## Validation

Confirm rule appears under Lifecycle management.

## Constraints

- Delete rules can remove data.
- The rule may not run immediately.
- Soft delete may allow recovery during retention period.

## Do's

- Apply delete rules only to temp data.
- Use container or prefix filters.

## Don'ts

- Do not apply delete rules to raw or production data without approval.
- Do not use delete rules without understanding impact.

## Learning Outcome

You learned how to automate temporary data cleanup.

---

# Exercise 20: Understand Storage Account Access Keys

## Level

Intermediate

## Goal

Understand access keys and why they are sensitive.

## Beginner Explanation

Access keys are like master passwords for a storage account.

Anyone with the key may get powerful access depending on how it is used.

## Problem Statement

Locate storage account access keys and understand why they should not be shared.

## Required Dataset

No dataset required.

## Steps

1. Open your storage account.
2. Search for **Access keys**.
3. Open the Access keys page.
4. Observe:
   - key1
   - key2
   - connection string
5. Do not copy or share these values.

## Expected Output

You should know where access keys are found.

## Validation

Explain why access keys are sensitive.

## Constraints

- Access keys must be protected.
- If leaked, they should be rotated.
- In production, prefer Microsoft Entra ID/RBAC where possible.

## Do's

- Keep keys secret.
- Rotate keys periodically in real projects.
- Use Key Vault for production secrets.

## Don'ts

- Do not commit keys to GitHub.
- Do not share keys in screenshots.
- Do not paste keys in notebooks or public docs.

## Learning Outcome

You learned why account keys must be handled carefully.

---

# Exercise 21: Create an Azure File Share

## Level

Intermediate

## Goal

Understand Azure Files.

## Beginner Explanation

Azure Files is used for cloud-based file shares.

It is different from Blob Storage. Blob Storage is object storage. Azure Files is more like a shared folder.

## Problem Statement

Create a file share and upload a text file.

## Required Dataset

Create:

```text
notes.txt
```

Content:

```text
This file is uploaded to Azure File Share.
Azure Files is different from Blob Storage.
```

## Steps

1. Open your storage account.
2. Click **File shares**.
3. Click **+ File share**.
4. Enter file share name:

```text
trainingfileshare
```

5. Select standard/default tier suitable for practice.
6. Click **Create**.
7. Open the file share.
8. Upload `notes.txt`.

## Expected Output

A file share named `trainingfileshare` should contain `notes.txt`.

## Validation

Open the file share and confirm the file exists.

## Constraints

- Azure Files pricing differs from Blob Storage.
- Mounting a file share needs additional steps not covered here.

## Do's

- Use Azure Files for shared folder use cases.
- Use Blob Storage for object/data lake use cases.

## Don'ts

- Do not confuse Azure Files with Blob Storage.
- Do not create large file shares for basic practice.

## Learning Outcome

You learned the basic use of Azure Files.

---

# Exercise 22: Create a Queue and Add a Message

## Level

Intermediate

## Goal

Understand Queue Storage.

## Beginner Explanation

A queue stores messages.

Applications use queues to communicate asynchronously.

Example message:

```text
New order file uploaded for processing
```

## Problem Statement

Create a queue and add one message.

## Required Dataset

No file dataset required.

## Steps

1. Open your storage account.
2. Click **Queues**.
3. Click **+ Queue**.
4. Queue name:

```text
order-processing-queue
```

5. Click **Create**.
6. Open the queue.
7. Click **Add message**.
8. Enter:

```text
New order file uploaded for processing
```

9. Save/add the message.

## Expected Output

A queue should be created with one message.

## Validation

Open the queue and confirm the message exists.

## Constraints

- Queue messages are not for large files.
- Store file path/event details in messages, not full datasets.

## Do's

- Use queues for application communication.
- Keep messages small.

## Don'ts

- Do not store large data in queue messages.
- Do not use queues as a database.

## Learning Outcome

You learned the basic use of Queue Storage.

---

# Exercise 23: Create a Table

## Level

Intermediate

## Goal

Understand Azure Table Storage.

## Beginner Explanation

Azure Table Storage is a NoSQL key-value style service.

It is not the same as SQL tables.

## Problem Statement

Create a table named `StudentStatus`.

## Required Dataset

No dataset required.

## Steps

1. Open your storage account.
2. Click **Tables**.
3. Click **+ Table**.
4. Table name:

```text
StudentStatus
```

5. Click **Create**.

## Expected Output

A table named `StudentStatus` should be created.

## Validation

Confirm the table appears under Tables.

## Constraints

- Table Storage is NoSQL.
- It does not support relational joins like SQL databases.

## Do's

- Use tables for simple entity data.
- Learn PartitionKey and RowKey later.

## Don'ts

- Do not confuse Azure Table Storage with SQL Server tables.
- Do not use it for complex relational analytics.

## Learning Outcome

You learned the basic use of Azure Table Storage.

---

# Exercise 24: Monitor Storage Account Metrics

## Level

Intermediate

## Goal

Check storage account usage and activity.

## Beginner Explanation

In real projects, teams monitor storage usage, transactions, availability, ingress, egress, and errors.

## Problem Statement

Open storage account metrics and observe usage.

## Required Dataset

Use files uploaded in previous exercises.

## Steps

1. Open your storage account.
2. Go to **Monitoring**.
3. Click **Metrics**.
4. Observe metrics such as:
   - Used capacity
   - Transactions
   - Ingress
   - Egress
   - Availability
5. Change time range to last 24 hours.

## Expected Output

Charts should show storage account activity.

## Validation

Identify at least two metrics and explain what they mean.

## Constraints

- Metrics may take time to appear.
- Newly created storage accounts may show little data.

## Do's

- Check metrics after upload/download.
- Use monitoring for troubleshooting.

## Don'ts

- Do not ignore monitoring in real projects.
- Do not assume no chart means no cost.

## Learning Outcome

You learned basic storage monitoring.

---

# Exercise 25: Create Data Lake Storage Gen2 Account

## Level

Advanced

## Goal

Create a storage account with hierarchical namespace enabled.

## Beginner Explanation

Azure Data Lake Storage Gen2 is Blob Storage with hierarchical namespace enabled.

It is commonly used for big data and analytics.

## Problem Statement

Create a new storage account for Data Lake practice.

## Required Dataset

No dataset required.

## Steps

1. Go to Azure Portal.
2. Search **Storage accounts**.
3. Click **Create**.
4. Select resource group:

```text
rg-storage-practice
```

5. Enter a unique name:

```text
dl<yourname>demo001
```

6. Select region:

```text
Central India
```

7. Performance:

```text
Standard
```

8. Redundancy:

```text
LRS
```

9. Go to **Advanced**.
10. Enable:

```text
Hierarchical namespace
```

11. Click **Review + create**.
12. Click **Create**.

## Expected Output

A Data Lake-enabled storage account should be created.

## Validation

Open the storage account and confirm hierarchical namespace is enabled.

## Constraints

- Hierarchical namespace is usually enabled during storage account creation.
- Some features may behave differently with hierarchical namespace enabled.
- Use a separate account for this exercise.

## Do's

- Use ADLS Gen2 for analytics scenarios.
- Use LRS for practice.

## Don'ts

- Do not create many data lake accounts unnecessarily.
- Do not use premium settings unless instructed.

## Learning Outcome

You learned how to create an ADLS Gen2-enabled storage account.

---

# Exercise 26: Create Data Lake Containers for Medallion Architecture

## Level

Advanced

## Goal

Create containers for medallion architecture.

## Beginner Explanation

Medallion architecture organizes data into layers:

- Landing: files just arrived
- Bronze: raw ingested data
- Silver: cleaned data
- Gold: business-ready data
- Archive: old data

## Problem Statement

Create medallion containers in the ADLS Gen2 account.

## Required Dataset

No dataset required.

## Steps

1. Open your ADLS Gen2 storage account.
2. Go to **Containers**.
3. Create:

```text
landing
bronze
silver
gold
archive
```

4. Keep each container private.

## Expected Output

Five containers should be created.

## Validation

Confirm all containers are visible.

## Constraints

- Container names must be lowercase.
- Containers should remain private.

## Do's

- Use clear layer names.
- Keep containers private.

## Don'ts

- Do not store all data randomly in one container.
- Do not make data lake containers public.

## Learning Outcome

You learned how containers can represent data lake layers.

---

# Exercise 27: Create Directory Structure in ADLS Gen2

## Level

Advanced

## Goal

Create structured data lake paths.

## Beginner Explanation

Data lakes usually organize files by domain, entity, and date.

Example:

```text
landing/ecommerce/orders/year=2026/month=06/day=19/orders.csv
```

## Problem Statement

Upload e-commerce files into proper directory paths.

## Required Dataset

Use:

```text
customers.csv
orders.csv
products.json
sales_2026_06_19.csv
```

## Required Folder Structure

```text
landing/ecommerce/customers/year=2026/month=06/day=19/customers.csv
landing/ecommerce/orders/year=2026/month=06/day=19/orders.csv
landing/ecommerce/products/year=2026/month=06/day=19/products.json
landing/ecommerce/sales/year=2026/month=06/day=19/sales_2026_06_19.csv
```

## Steps

1. Open ADLS Gen2 storage account.
2. Open the `landing` container.
3. Create directory:

```text
ecommerce
```

4. Inside `ecommerce`, create:

```text
customers
orders
products
sales
```

5. Inside each subject folder, create:

```text
year=2026/month=06/day=19
```

6. Upload files to their respective paths.

## Expected Output

Files should be stored in correct entity/date paths.

## Validation

Navigate through directories and confirm each file is in the correct location.

## Constraints

- Do not upload files directly into landing root.
- Use consistent folder naming.
- Avoid spaces in folder names.

## Do's

- Use structured paths.
- Use entity and date-based folders.

## Don'ts

- Do not use inconsistent names like `jun`, `month6`, `19-06-2026`.
- Do not mix unrelated files in one folder.

## Learning Outcome

You learned how to design data lake folder structure.

---

# Exercise 28: Understand RBAC for Blob Data

## Level

Advanced

## Goal

Understand role-based access control.

## Beginner Explanation

RBAC means Role-Based Access Control.

Instead of giving everyone full access, assign only the required role.

Common roles:

| Role | Purpose |
|---|---|
| Storage Blob Data Reader | Read blob data |
| Storage Blob Data Contributor | Read, upload, update, delete blob data |
| Storage Blob Data Owner | Full data control and permission management |

## Problem Statement

Explore where storage roles are assigned.

## Required Dataset

No dataset required.

## Steps

1. Open your storage account.
2. Go to **Access Control (IAM)**.
3. Click **Role assignments**.
4. Observe existing roles.
5. Do not modify roles unless trainer instructs.

## Optional Trainer Demo

Trainer may assign `Storage Blob Data Reader` to one student and show that the student can read data but cannot upload or delete.

## Expected Output

Students should understand where RBAC is managed.

## Validation

Explain the difference between Reader and Contributor.

## Constraints

- Students may not have permission to assign roles.
- RBAC changes can take time to apply.
- Resource management roles and data access roles are different.

## Do's

- Use least privilege.
- Give only required access.

## Don'ts

- Do not give Owner role casually.
- Do not use account keys when RBAC is enough.

## Learning Outcome

You learned the basics of Azure Storage RBAC.

---

# Exercise 29: Review Storage Cost and Usage

## Level

Beginner

## Goal

Check storage-related cost and usage.

## Beginner Explanation

Cloud resources can generate cost based on storage size, transactions, redundancy, access tier, data transfer, and retained deleted/versioned data.

## Problem Statement

Check Cost Management after completing storage exercises.

## Required Dataset

No dataset required.

## Steps

1. Open Azure Portal.
2. Search for **Cost Management + Billing**.
3. Open **Cost Management**.
4. Open **Cost analysis**.
5. Filter by resource group if possible:

```text
rg-storage-practice
```

6. Review current cost and usage.

## Expected Output

You should be able to view cost or usage details.

## Validation

Check whether storage resources appear in cost analysis.

## Constraints

- Cost data may take time to appear.
- New resources may not show cost immediately.
- Free credits do not mean usage should be ignored.

## Do's

- Check cost regularly.
- Use budget alerts.

## Don'ts

- Do not ignore cost because the account is free.
- Do not leave resources running unnecessarily.

## Learning Outcome

You learned how to review cost visibility.

---

# Exercise 30: Cleanup Practice Resources

## Level

Beginner

## Goal

Delete practice resources safely.

## Beginner Explanation

Cleanup is one of the most important cloud habits.

If you no longer need resources, delete them to avoid cost.

## Problem Statement

Delete the practice resource group after trainer approval.

## Required Dataset

No dataset required.

## Steps

1. Go to Azure Portal.
2. Search for **Resource groups**.
3. Open:

```text
rg-storage-practice
```

4. Review resources inside it.
5. Confirm with your trainer.
6. Click **Delete resource group**.
7. Type:

```text
rg-storage-practice
```

8. Click **Delete**.

## Expected Output

The resource group and all resources inside it should be deleted.

## Validation

Search for `rg-storage-practice` again. It should not appear after deletion completes.

## Constraints

- Deletion can take a few minutes.
- Deleted resources may not be recoverable.
- Do not delete shared or production resource groups.

## Do's

- Delete practice resources after the lab.
- Confirm before deleting.

## Don'ts

- Do not delete resources used by others.
- Do not leave paid resources active.

## Learning Outcome

You learned safe Azure cleanup practice.

---

# Mini Project

## Project Title

Build a Simple E-commerce Data Lake Using Azure Storage Account

## Difficulty

Intermediate to Advanced

## Business Scenario

An e-commerce company receives daily files from multiple systems:

- Customer master from CRM
- Orders from order management system
- Product catalog from inventory system
- Sales transactions from billing system

The company wants to store these files in a structured Azure Data Lake layout.

You are asked to create the storage structure and upload files properly.

---

## Project Requirements

### 1. Create an ADLS Gen2 Storage Account

Create a storage account with hierarchical namespace enabled.

### 2. Create Containers

```text
landing
bronze
silver
gold
archive
```

### 3. Upload Files to Landing Zone

Use this folder structure:

```text
landing/ecommerce/customers/year=2026/month=06/day=19/customers.csv
landing/ecommerce/orders/year=2026/month=06/day=19/orders.csv
landing/ecommerce/products/year=2026/month=06/day=19/products.json
landing/ecommerce/sales/year=2026/month=06/day=19/sales_2026_06_19.csv
```

### 4. Add Metadata

Add metadata to each file.

| Key | Value |
|---|---|
| source_system | ecommerce |
| data_layer | landing |
| uploaded_by | student |
| file_frequency | daily |

### 5. Enable Data Protection

Enable:

- Soft delete
- Blob versioning if supported in your selected configuration

### 6. Create Lifecycle Rule

Create a lifecycle rule:

```text
Move landing files older than 30 days to Cool tier.
```

### 7. Generate SAS

Generate read-only SAS URL for:

```text
customers.csv
```

Expiry:

```text
30 minutes
```

### 8. Monitor

Check metrics for:

- Used capacity
- Transactions
- Ingress
- Egress

### 9. Cleanup

Delete resources after trainer approval.

---

## Expected Final Layout

```text
landing/
  ecommerce/
    customers/
      year=2026/
        month=06/
          day=19/
            customers.csv
    orders/
      year=2026/
        month=06/
          day=19/
            orders.csv
    products/
      year=2026/
        month=06/
          day=19/
            products.json
    sales/
      year=2026/
        month=06/
          day=19/
            sales_2026_06_19.csv

bronze/
silver/
gold/
archive/
```

---

## Project Learning Outcome

You learned how Azure Storage Account can be used as the foundation of a real data engineering data lake.

---

# Do's and Don'ts

## Do's

1. Use Standard performance for practice.
2. Use LRS redundancy for labs.
3. Keep containers private.
4. Use short-lived SAS tokens.
5. Use structured folder paths.
6. Use metadata for simple tracking.
7. Enable soft delete before testing delete recovery.
8. Enable versioning before testing overwrite recovery.
9. Use lifecycle filters carefully.
10. Delete resources after practice.
11. Check Cost Management regularly.
12. Use RBAC where possible in real projects.

## Don'ts

1. Do not choose Premium storage unnecessarily.
2. Do not choose GRS/GZRS for basic practice unless instructed.
3. Do not make containers public.
4. Do not upload sensitive personal data.
5. Do not commit SAS tokens or keys to GitHub.
6. Do not share account keys.
7. Do not create SAS tokens with long expiry.
8. Do not move active files to Archive tier.
9. Do not apply delete lifecycle rules without filters.
10. Do not forget cleanup.
11. Do not use random naming in real projects.
12. Do not use production resources for practice.

---

# Common Mistakes and Troubleshooting

| Problem | Possible Reason | Solution |
|---|---|---|
| Storage account name not accepted | Name already used globally | Choose a different unique name |
| Cannot create storage account | Missing permission or subscription issue | Check subscription and permissions |
| Upload button not visible | Wrong menu or no permission | Open container and check access |
| Blob URL does not open | Container is private | Use SAS URL or RBAC |
| SAS URL expired | Expiry time passed | Generate a new SAS |
| Cannot recover deleted blob | Soft delete was not enabled | Enable soft delete before future deletes |
| Version not visible | Versioning not enabled before overwrite | Enable versioning and repeat overwrite |
| Lifecycle rule not executing | File does not meet age condition | Wait or test with correct condition |
| Metrics are empty | Data not available yet | Wait and refresh later |
| Cost not visible | Billing data delay | Check after some time |
| Public access not allowed | Tenant/subscription policy blocks it | Use private access with SAS/RBAC |
| Cannot assign RBAC role | You do not have permission | Ask trainer/admin |

---

# Interview Questions

## Basic Questions

1. What is an Azure Storage Account?
2. What services are available inside a storage account?
3. What is Blob Storage?
4. What is a container?
5. What is a blob?
6. What is a resource group?
7. Why should storage account names be globally unique?
8. What is the difference between a container and a folder?
9. What is the purpose of the raw container?
10. Why do we use private containers?

## Intermediate Questions

11. What are access tiers?
12. When would you use Hot tier?
13. When would you use Cool tier?
14. What is Cold tier used for?
15. What is Archive tier?
16. What is LRS?
17. What is ZRS?
18. What is GRS?
19. What is SAS?
20. What is the difference between blob SAS and container SAS?
21. What is soft delete?
22. What is blob versioning?
23. What is lifecycle management?
24. Why should account keys be protected?
25. What is the difference between Azure Files and Blob Storage?

## Advanced Questions

26. What is ADLS Gen2?
27. What is hierarchical namespace?
28. Why is ADLS Gen2 preferred for data lakes?
29. What is medallion architecture?
30. What is the difference between RBAC and SAS?
31. What role is required to upload blobs?
32. What role is required only to read blobs?
33. Why should lifecycle delete rules use filters?
34. Why is Archive tier not suitable for frequently accessed data?
35. How would you design storage for a daily batch data pipeline?
36. What are the risks of public container access?
37. Why should SAS expiry be short?
38. What is the difference between authentication and authorization?
39. How does soft delete differ from versioning?
40. How can storage account monitoring help in production?

---

# Final Cleanup Checklist

Before closing the lab, complete this checklist:

| Task | Status |
|---|---|
| Checked containers | ☐ |
| Checked uploaded files | ☐ |
| Removed shared SAS URLs from notes/chats | ☐ |
| Reviewed lifecycle rules | ☐ |
| Checked Cost Management | ☐ |
| Confirmed with trainer before deleting | ☐ |
| Deleted `rg-storage-practice` if no longer needed | ☐ |
| Confirmed resource group deletion | ☐ |

---

# Final Summary

In this lab, you practiced:

```text
Resource group creation
Storage account creation
Container creation
File upload/download
Virtual folder paths
Blob properties
Metadata
Access tiers
SAS URLs
Public access checks
Soft delete
Blob versioning
Lifecycle management
Azure Files
Queue Storage
Table Storage
Monitoring
ADLS Gen2
Medallion architecture
RBAC concepts
Cost review
Cleanup
```

Key points to remember:

```text
Storage Account = Main storage resource
Container = Logical storage area
Blob = Actual file/object
SAS = Temporary access URL
Access Tier = Cost and access behavior
Soft Delete = Recovery after deletion
Versioning = Recovery after overwrite
Lifecycle Rule = Automated data movement or deletion
ADLS Gen2 = Blob Storage with hierarchical namespace
```

Always remember:

```text
Create only what is needed.
Keep containers private.
Use least privilege access.
Protect keys and SAS tokens.
Monitor cost.
Delete practice resources after use.
```

---

# References

1. Azure Storage account overview  
   https://learn.microsoft.com/en-us/azure/storage/common/storage-account-overview

2. Introduction to Azure Storage  
   https://learn.microsoft.com/en-us/azure/storage/common/storage-introduction

3. Azure Blob Storage access tiers  
   https://learn.microsoft.com/en-us/azure/storage/blobs/access-tiers-overview

4. Azure Blob Storage lifecycle management  
   https://learn.microsoft.com/en-us/azure/storage/blobs/lifecycle-management-overview

5. Azure Storage redundancy  
   https://learn.microsoft.com/en-us/azure/storage/common/storage-redundancy

6. Shared access signatures overview  
   https://learn.microsoft.com/en-us/azure/storage/common/storage-sas-overview

7. Authorize access to Azure Storage  
   https://learn.microsoft.com/en-us/azure/storage/common/authorize-data-access

8. Soft delete for blobs  
   https://learn.microsoft.com/en-us/azure/storage/blobs/soft-delete-blob-overview

9. Manage storage account access keys  
   https://learn.microsoft.com/en-us/azure/storage/common/storage-account-keys-manage
