# Azure IAM, RBAC, Policy, and Billing — Hands-on Guide

## Industry Use Case

A company named **EcomPro Retail** is building a data lake on Azure.

Different users need different levels of access:

| User / Team | Required Access |
|---|---|
| Data Engineer | Upload and download files in ADLS Gen2 |
| Auditor | Read and download files only |
| Admin | Manage the Azure resources |
| Finance Team | Track project cost |
| Governance Team | Ensure Azure rules are followed |

---

## Activity Goal

In this hands-on activity, you will understand how the following Azure concepts work together:

```text
Microsoft Entra ID
Tenant / Directory
Users
Guest Users
Groups
RBAC Roles
Management Groups
Subscriptions
Resource Groups
Azure Policy
Billing Profile
Invoice Section
ADLS Gen2 Storage Account
```

---

## Final Architecture

```text
Microsoft Entra Tenant / Directory
│
├── Users / Guest Users
│   ├── Admin user
│   ├── Data engineer user
│   └── Auditor user
│
├── Groups
│   ├── grp-data-engineers
│   └── grp-auditors
│
└── Azure Resource Hierarchy
    │
    └── Management Group: mg-training
        │
        └── Subscription: sub-ecompro-training
            │
            └── Resource Group: rg-ecompro-datalake
                │
                └── ADLS Gen2 Storage Account
                    │
                    └── Container: data
                        │
                        ├── raw/
                        ├── bronze/
                        ├── silver/
                        └── gold/
```

Billing structure:

```text
Billing Account
│
└── Billing Profile
    │
    └── Invoice Section: EcomPro Training Project
        │
        └── Subscription: sub-ecompro-training
```

---

## Prerequisites

Before starting, make sure you have:

```text
1. Azure account with active subscription
2. Permission to invite guest users or create users
3. Permission to create groups
4. Permission to assign RBAC roles
5. Permission to create resource groups and storage accounts
6. One test/student account for access testing
```

---

## Open Microsoft Entra ID

Go to:

```text
Azure Portal → Microsoft Entra ID
```

Microsoft Entra ID is Azure’s identity system.

It stores:

```text
Users
Guest users
Groups
Applications
Service principals
Managed identities
```

Important concept:

```text
Tenant / Directory = Identity boundary
```

A tenant is where users and groups exist.

---

## Invite an External User as Guest User

Go to:

```text
Microsoft Entra ID → Users → New user → Invite external user
```

Invite a test user, for example:

```text
student-demo@gmail.com
```

The invited user should accept the invitation.

After accepting, the user becomes a **Guest User** in your tenant.

Important:

```text
You are not giving access to the whole external tenant.
You are inviting one specific external user into your tenant.
```

---

## Create User Groups

Go to:

```text
Microsoft Entra ID → Groups → New group
```

Create two groups:

```text
grp-data-engineers
grp-auditors
```

Add users:

```text
grp-data-engineers → student-demo@gmail.com
grp-auditors → auditor/test user
```

Why groups are useful:

```text
Instead of assigning access to users one by one,
you can assign access to a group.
All users inside that group receive the assigned permission.
```

---

## Create a Resource Group

Go to:

```text
Resource groups → Create
```

Use:

```text
Resource Group Name: rg-ecompro-datalake
Region: Central India
```

A resource group is a project-level container for Azure resources.

Example:

```text
rg-ecompro-datalake
│
├── Storage Account
├── Key Vault
└── Data Factory
```

---

## Create ADLS Gen2 Storage Account

Go to:

```text
Storage accounts → Create
```

Use the following settings:

```text
Resource Group: rg-ecompro-datalake
Storage Account Name: stecomprodatalake<unique-number>
Performance: Standard
Redundancy: LRS
Hierarchical Namespace: Enabled
```

Important:

```text
ADLS Gen2 = Blob Storage + Hierarchical Namespace
```

ADLS Gen2 is commonly used for data lake and data engineering workloads.

---

## Create Container and Folder Structure

Create one container:

```text
data
```

Inside the `data` container, create these directories:

```text
raw/
bronze/
silver/
gold/
```

Upload a sample file:

```text
raw/customers/customers.csv
```

In ADLS Gen2, folders/directories are real because **Hierarchical Namespace** is enabled.

---

## Assign RBAC Roles

RBAC means **Role-Based Access Control**.

RBAC answers:

```text
Who can do what, and where?
```

RBAC formula:

```text
User / Group + Role + Scope = Access
```

Example:

```text
grp-data-engineers + Storage Blob Data Contributor + data container = upload/download access
```

### Assign Reader Role for Portal Visibility

Scope:

```text
Storage Account
```

Assign:

```text
Reader → grp-data-engineers
Reader → grp-auditors
```

Purpose:

```text
Reader role allows users to see the Azure resource in the portal.
```

### Assign Data Engineer Access

Scope:

```text
Container: data
```

Assign:

```text
Storage Blob Data Contributor → grp-data-engineers
```

This allows data engineers to:

```text
List files
Read files
Download files
Upload files
Overwrite files
Delete files
```

### Assign Auditor Access

Scope:

```text
Container: data
```

Assign:

```text
Storage Blob Data Reader → grp-auditors
```

This allows auditors to:

```text
List files
Read files
Download files
```

Auditors should not be able to:

```text
Upload files
Overwrite files
Delete files
```

---

## Test Data Engineer Access

Login using the data engineer test account.

Steps:

```text
1. Open Azure Portal
2. Switch to the correct directory/tenant
3. Open the storage account
4. Open the data container
5. Download customers.csv
6. Upload a new file inside raw/
```

Expected result:

```text
Download should work.
Upload should work.
```

Reason:

```text
The user is part of grp-data-engineers.
The group has Storage Blob Data Contributor access.
```

---

## Test Auditor Access

Login using the auditor test account.

Steps:

```text
1. Open Azure Portal
2. Switch to the correct directory/tenant
3. Open the storage account
4. Open the data container
5. Download customers.csv
6. Try uploading a new file
```

Expected result:

```text
Download should work.
Upload should fail.
```

Reason:

```text
The user is part of grp-auditors.
The group has Storage Blob Data Reader access only.
```

---

## Understand Azure Policy

Azure Policy is different from RBAC.

RBAC answers:

```text
Who can do what?
```

Azure Policy answers:

```text
What is allowed or not allowed?
```

Example policies:

```text
Resources can be created only in Central India.
Storage account public access must be disabled.
Only selected VM sizes are allowed.
Only LRS storage is allowed for training subscriptions.
```

Example:

```text
RBAC: User can create storage accounts.
Policy: But storage accounts can be created only in Central India.
```

So even if a user has permission, Azure Policy can still restrict what is allowed.

---

## Understand Management Groups

Management groups are used to organize subscriptions.

Example:

```text
Management Group: mg-training
│
└── Subscription: sub-ecompro-training
```

In large companies, management groups may look like this:

```text
Root Management Group
│
├── Production
│   ├── Prod Subscription 1
│   └── Prod Subscription 2
│
├── Development
│   ├── Dev Subscription 1
│   └── Dev Subscription 2
│
└── Training
    └── Student Lab Subscription
```

Management groups are useful for:

```text
Organizing subscriptions
Applying policies to multiple subscriptions
Applying RBAC permissions to multiple subscriptions
Governance and compliance
```

For small practice accounts, you may only have one subscription, so management groups may not be heavily used.

---

## Understand Billing Profile and Invoice Section

Go to:

```text
Cost Management + Billing
```

Billing hierarchy usually looks like this:

```text
Billing Account
│
└── Billing Profile
    │
    └── Invoice Section
        │
        └── Subscription
```

Meaning:

| Concept | Meaning |
|---|---|
| Billing Account | Who pays Microsoft |
| Billing Profile | Invoice and payment grouping |
| Invoice Section | Cost grouping inside an invoice |
| Subscription | Where Azure resources are created and costs are generated |

Example:

```text
Storage Account
   ↓ cost generated
Resource Group
   ↓ belongs to
Subscription
   ↓ billed under
Invoice Section
   ↓ belongs to
Billing Profile
   ↓ belongs to
Billing Account
```

Important:

```text
IAM controls access.
Billing controls invoices and cost tracking.
They are connected through the subscription.
```

---

## Check Cost

Go to:

```text
Cost Management → Cost analysis
```

Filter by:

```text
Resource Group: rg-ecompro-datalake
```

Observe the cost of resources created during this activity.

Important:

```text
Resources generate cost.
Cost rolls up to the subscription.
Subscription is linked to billing profile and invoice section.
```

---

## Cleanup

After completing the activity:

```text
1. Remove role assignments if they are no longer required.
2. Remove guest users if they are no longer required.
3. Delete the resource group rg-ecompro-datalake.
4. Check Cost Management.
```

Deleting the resource group removes the resources inside it.

---

## Key Concepts Summary

### Identity

```text
Microsoft Entra ID = Identity system
Tenant / Directory = Organization identity boundary
User = Internal identity
Guest user = External identity invited into tenant
Group = Collection of users
```

### Access

```text
RBAC = Role-Based Access Control
Role = Permission set
Scope = Where permission applies
Role assignment = User/group + role + scope
```

### Resource Hierarchy

```text
Management Group
→ Subscription
→ Resource Group
→ Resource
```

### Billing Hierarchy

```text
Billing Account
→ Billing Profile
→ Invoice Section
→ Subscription
```

### Governance

```text
RBAC = Who can do what
Policy = What is allowed or denied
Billing = Who pays and how cost is grouped
```

---

## Common Role Examples

| Role | Purpose |
|---|---|
| Reader | View Azure resources in portal |
| Contributor | Create and manage Azure resources |
| Owner | Full access including role assignment |
| Storage Blob Data Reader | Read/list/download blob data |
| Storage Blob Data Contributor | Read/upload/delete blob data |
| Storage Blob Data Owner | Full blob data access including ACL management |

---

## Final Understanding

Microsoft Entra ID manages identities.

RBAC assigns permissions to those identities.

Management groups, subscriptions, resource groups, and resources organize where Azure services live.

Azure Policy enforces rules.

Billing account, billing profile, and invoice section organize cost and invoices.

Final one-liner:

```text
Entra ID manages who exists.
RBAC manages what they can do.
Management groups and subscriptions organize resources.
Policies enforce rules.
Billing profiles and invoice sections organize cost.
```
