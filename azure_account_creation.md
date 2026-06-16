# Azure Account Creation Guide

> **Purpose:** This guide will help you create your Microsoft Azure account safely and correctly for hands-on cloud practice.
>
> **Audience:** Students starting Azure, Cloud, or Data Engineering practical sessions.

---

## Table of Contents

1. [Why Do We Need an Azure Account?](#1-why-do-we-need-an-azure-account)
2. [Things You Need Before Starting](#2-things-you-need-before-starting)
3. [Important Note About Card Verification](#3-important-note-about-card-verification)
4. [Azure Free Account Benefits](#4-azure-free-account-benefits)
5. [Step-by-Step Azure Account Creation](#5-step-by-step-azure-account-creation)
6. [Open Azure Portal](#6-open-azure-portal)
7. [Check Your Subscription](#7-check-your-subscription)
8. [Check Free Credit and Billing](#8-check-free-credit-and-billing)
9. [Create a Budget Alert](#9-create-a-budget-alert)
10. [First Practice Task: Create a Resource Group](#10-first-practice-task-create-a-resource-group)
11. [Recommended Practice Rule](#11-recommended-practice-rule)
12. [How to Delete Practice Resources](#12-how-to-delete-practice-resources)
13. [Services Students Should Avoid Without Instructions](#13-services-students-should-avoid-without-instructions)
14. [Common Mistakes to Avoid](#14-common-mistakes-to-avoid)
15. [Azure for Students Option](#15-azure-for-students-option)
16. [Final Checklist](#16-final-checklist)
17. [Summary](#17-summary)

---

# 1. Why Do We Need an Azure Account?

In this course, we will use **Microsoft Azure** for hands-on practice.

You will use Azure to learn and practice topics such as:

- Creating **Resource Groups**
- Creating **Storage Accounts**
- Working with **containers and files**
- Practicing cloud-based **Data Engineering services**
- Understanding **billing, subscriptions, and resource management**

Azure provides a free account option for new users. This is usually enough for basic learning, but only if we use it carefully and monitor the cost regularly.

---

# 2. Things You Need Before Starting

Before creating your Azure account, keep the following things ready:

| Requirement | Why It Is Needed |
|---|---|
| Microsoft account | To sign in to Azure |
| Mobile number | For OTP verification |
| Debit/Credit card | For identity verification |
| Address details | For billing profile setup |
| Internet connection | To access Azure Portal |
| Browser | Chrome, Edge, or Firefox is recommended |

> **Note:** If you already have an Outlook, Hotmail, or Live email account, you can use it as your Microsoft account.

---

# 3. Important Note About Card Verification

Azure usually asks for a **debit card or credit card** during free account creation.

This card is mainly used for **identity verification**.

For the free account:

- Azure may temporarily verify your card.
- A small verification hold may appear.
- This verification hold is generally reversed later.
- You should not be charged automatically while you stay within free limits.
- You must still monitor your usage carefully.

> **Important:** Do not create paid resources carelessly. Cloud services can create charges if resources are left running or if free limits are crossed.

---

# 4. Azure Free Account Benefits

A new Azure free account usually provides:

| Benefit | Description |
|---|---|
| Free credit for first 30 days | You can use this credit to try Azure services. |
| Some services free for 12 months | Selected services are available free within fixed limits. |
| Some services always free | Some services remain free within monthly usage limits. |

This means you can practice basic Azure services without paying, but only if you stay within the allowed free limits.

---

# 5. Step-by-Step Azure Account Creation

## Step 1: Search for Azure Free Account

Open your browser and search:

```text
Azure free account Microsoft
```

Open the official Microsoft Azure website.

> **Warning:** Do not use third-party websites for account creation. Always use the official Microsoft Azure website.

---

## Step 2: Click on Start Free

On the Azure free account page:

1. Click **Start free**.
2. Sign in using your Microsoft account.
3. If you do not have a Microsoft account, create one first.

Example Microsoft accounts:

```text
yourname@outlook.com
yourname@hotmail.com
yourname@gmail.com registered as a Microsoft account
```

---

## Step 3: Fill Basic Profile Details

Azure will ask for your personal details.

Fill the details carefully:

- First name
- Last name
- Country/Region
- Email address
- Phone number
- Address
- City
- State
- Postal code

> **Note:** Use correct details because these details are connected with your Azure billing profile.

---

## Step 4: Verify Your Mobile Number

Azure will ask you to verify your phone number.

You may see options like:

- **Text me**
- **Call me**

Choose one option and enter the OTP received on your mobile number.

---

## Step 5: Verify Your Card

Azure will now ask for card details.

Enter the following details:

- Card number
- Expiry date
- CVV
- Name on card

> **Remember:** This card is mainly used for verification. Still, you must control your usage and monitor billing regularly.

---

## Step 6: Accept Terms and Conditions

Read and accept the required terms:

- Microsoft Customer Agreement
- Privacy statement
- Azure subscription terms

Then click **Sign up**.

---

## Step 7: Wait for Account Setup

Azure will now create your account.

It may create:

- Azure account
- Billing profile
- Free Trial subscription
- Default directory / tenant

After setup, you will be redirected to the Azure Portal.

---

# 6. Open Azure Portal

Once the account is ready, open:

```text
portal.azure.com
```

Sign in using the same Microsoft account.

The Azure Portal is the main place where we create and manage Azure resources.

---

# 7. Check Your Subscription

After logging into Azure Portal:

1. Go to the search bar at the top.
2. Search **Subscriptions**.
3. Open **Subscriptions**.
4. Check whether your subscription is active.

You may see a subscription name like:

```text
Azure Free Trial
```

or

```text
Free Trial
```

The status should be:

```text
Active
```

---

# 8. Check Free Credit and Billing

To check your free credit:

1. Search **Cost Management + Billing**.
2. Open it.
3. Select your billing scope or subscription.
4. Check your remaining credit.
5. Check the expiry date of your free credit.

> **Good Practice:** Check this regularly during the course.

---

# 9. Create a Budget Alert

This step is strongly recommended for every student.

A budget alert helps you get an email when your Azure usage crosses a certain amount.

## Steps to Create a Budget Alert

1. Open Azure Portal.
2. Search **Cost Management + Billing**.
3. Open **Cost Management**.
4. Click **Budgets**.
5. Click **Add**.
6. Enter budget details.

Example budget:

```text
Budget name: student-practice-budget
Budget amount: ₹500 or ₹1000
Alert at: 50%, 80%, 100%
Email: your email address
```

This will help you avoid unexpected usage.

---

# 10. First Practice Task: Create a Resource Group

A **Resource Group** is like a folder where Azure resources are kept.

For practice, we will create one resource group and keep all learning resources inside it.

## Steps

1. Open Azure Portal.
2. Search **Resource groups**.
3. Click **Create**.
4. Select your subscription.
5. Enter resource group name:

```text
rg-practice-01
```

6. Select region:

```text
Central India
```

or

```text
East US
```

7. Click **Review + create**.
8. Click **Create**.

Now your first Azure resource group is ready.

---

# 11. Recommended Practice Rule

During this course, follow this rule:

> **Create all practice resources inside one resource group.**

Example:

```text
rg-practice-01
```

## Why should we use one resource group?

Because after practice, you can delete the complete resource group and remove all resources inside it.

This helps avoid unwanted charges.

---

# 12. How to Delete Practice Resources

After completing practice:

1. Go to **Resource groups**.
2. Open your practice resource group.
3. Click **Delete resource group**.
4. Type the resource group name for confirmation.
5. Click **Delete**.

Example:

```text
rg-practice-01
```

Deleting the resource group deletes all resources inside it.

> **Important:** Always delete unused practice resources after completing the lab.

---

# 13. Services Students Should Avoid Without Instructions

Some Azure services may create higher charges if not managed properly.

Do not create the following unless your trainer specifically asks you to:

- Large virtual machines
- Azure Databricks premium clusters
- NAT Gateway
- Application Gateway
- Azure Firewall
- High-performance databases
- Large storage accounts with huge data
- Public IPs left unused
- Load balancers
- Kubernetes clusters

For our basic learning, we will start with simple and controlled resources.

---

# 14. Common Mistakes to Avoid

| Mistake | Why It Is a Problem |
|---|---|
| Creating many resources randomly | Can increase cost |
| Forgetting to delete resource groups | Resources may keep running |
| Creating large VMs | High cost |
| Upgrading without understanding billing | May enable paid usage |
| Ignoring Cost Management | You may not notice usage |
| Using different resource groups for every lab | Cleanup becomes difficult |
| Leaving Databricks clusters running | Can consume credit quickly |

---

# 15. Azure for Students Option

If you are a student and do not have a debit or credit card, check this option:

```text
Azure for Students
```

Search on Google:

```text
Azure for Students Microsoft
```

This option may allow eligible students to create an Azure account without a credit card.

You usually need a valid student email or student verification.

---

# 16. Final Checklist

Before coming to the next practical session, make sure you have completed this checklist:

| Task | Status |
|---|---|
| Microsoft account created | ☐ |
| Azure free account created | ☐ |
| Mobile number verified | ☐ |
| Card verification completed | ☐ |
| Azure Portal login successful | ☐ |
| Subscription status checked | ☐ |
| Cost Management opened | ☐ |
| Budget alert created | ☐ |
| Practice resource group created | ☐ |
| You know how to delete resource group | ☐ |

---

# 17. Summary

To start Azure practice, you need to:

1. Create a Microsoft account.
2. Open the Azure free account page.
3. Sign in.
4. Fill profile details.
5. Verify mobile number.
6. Verify card.
7. Complete signup.
8. Open Azure Portal.
9. Check subscription.
10. Create a budget alert.
11. Create one practice resource group.
12. Delete resources after practice.

---

## Final Reminder

> Cloud learning requires cost awareness.
>
> Create only what is needed.
>
> Delete what is not required.
>
> Check billing regularly.
