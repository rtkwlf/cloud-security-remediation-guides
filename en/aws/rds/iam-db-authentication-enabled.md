# AWS / RDS / DS IAM Database Authentication Enabled

## Quick Info

| | |
|-|-|
| **Plugin Title** | DS IAM Database Authentication Enabled |
| **Cloud** | AWS |
| **Category** | RDS |
| **Description** | Ensures IAM Database Authentication is enabled for RDS database instances to manage database access |
| **More Info** | AWS Identity and Access Management (IAM) can be used to authenticate to your RDS DB instances. |
| **AWS Link** | https://docs.aws.amazon.com/neptune/latest/userguide/iam-auth.html |
| **Recommended Action** | Modify the PostgreSQL and MySQL type RDS instances to enable IAM database authentication. |


## Introduction

Amazon RDS supports **IAM database authentication** for MySQL, MariaDB, and PostgreSQL. With IAM authentication, you can use AWS IAM users or roles (via tokens) instead of static passwords to connect to your database, improving credential management and security.

By default, IAM database authentication is **disabled** on new and existing instances. To use it, you must **modify** the DB instance and enable IAM authentication. 

This guide walks you through enabling IAM authentication via the AWS Management Console and verifying that it’s active.


## Detailed Remediation Steps

### 1. Open the Amazon RDS Console and locate the target DB instance  
- Sign in to the AWS Management Console.  
- Navigate to **Amazon RDS**.  
- In the left navigation pane, click **Databases**.  
- Choose the MySQL or PostgreSQL instance you want to enable IAM authentication for.

### 2. Modify the DB instance to enable IAM authentication  
- With the instance selected, click **Actions → Modify**.  
- Scroll to the **Database authentication** section.  
- Select **Password and IAM database authentication**. (This enables IAM authentication alongside password authentication.)
- Optionally, enable **IAM DB authentication error logging** to CloudWatch under **Log exports** (choose `iam-db-auth-error`).
- Click **Continue** to proceed.

### 3. Choose when to apply the change  
- On the summary screen, choose whether to **Apply immediately** (causing downtime during change) or wait for the **next maintenance window**.
- Confirm by clicking **Modify DB instance**.

### 4. Wait for the change to complete  
- The DB instance status will transition (e.g. to `modifying`).  
- Wait until the status becomes **Available**—this means IAM authentication is enabled.


## Verification

- In the RDS Console, select the instance and view its **Configuration / Details**. Confirm that **Database authentication** shows **Password and IAM** (or similar) rather than just password-only.  
- Re-run your detection script. The instance should now report as having IAM Database Authentication enabled (i.e. no longer flagged as non‑compliant).  
- Optionally, test connecting via IAM token:
  - For **MySQL**: create a user account in the database using the `AWSAuthenticationPlugin` and test connecting using a generated token. 
  - For **PostgreSQL**: assign the `rds_iam` role to a database user (via `GRANT rds_iam`) and test connecting using an IAM-generated token.
