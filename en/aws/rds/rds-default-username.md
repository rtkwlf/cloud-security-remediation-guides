[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)

# AWS / RDS / RDS Instance Default Master Username

## Quick Info

| | |
|-|-|
| **Plugin Title** | RDS Instance Default Master Username |
| **Cloud** | AWS |
| **Category** | RDS |
| **Description** | Ensures RDS instance does not have a default master username |
| **More Info** | By default, RDS uses the default master username which has the maximum permissions for your instance. RDS instances should be configured to use unique username to ensure security. |
| **AWS Link** | https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CreateDBInstance.html |
| **Recommended Action** | Create a new RDS instance with the desired username, and migrate the database to the new instance. |


## Introduction

Amazon RDS requires you to specify the master username only at instance creation. Once an RDS instance is launched, you **cannot** change the master username.  
To address the case where a default or generic master username is used (e.g. `admin`, `postgres`), the remediation is to **create a new RDS instance** with a custom master username and **migrate** the databases (and related users/permissions) from the old instance to the new instance.  

This ensures better security posture (unique admin account) while preserving existing data and configurations.


## Detailed Remediation Steps

### 1. Choose desired configuration for new RDS instance

1. In the **RDS Console**, navigate to **Databases → Create database**.  
2. Select the same DB engine, version, instance class, storage, networking, parameter group, and option group settings as (or compatible with) the source instance.  
3. In the **Settings** section, set a **custom master username** (i.e. not the default). Provide a strong password and any optional configurations (e.g. backup retention, encryption, maintenance window).  
4. Launch the new instance and wait until its status becomes **Available**.

### 2. Migrate databases and schema from the old instance to the new one

There are multiple migration approaches. Choose the one that best fits your downtime tolerance and data size:

#### Option A: Backup / Restore (for engines that support it)

- On the source instance, export or back up all user databases (e.g. using native backup tools, `mysqldump` for MySQL, or SQL backup for SQL Server).  
- Copy the backup files to a location accessible from the new instance (e.g. via S3, EC2, or network).  
- Restore/import the backup files into the new instance, restoring schemas, data, stored procedures, etc.

#### Option B: Use AWS Database Migration Service (DMS) for continuous replication

- In the AWS console, open DMS and create a **replication instance**.  
- Define the **source endpoint** pointing to the old RDS instance.  
- Define the **target endpoint** pointing to the new RDS instance.  
- Create a **migration task** configured for **full load + ongoing replication (CDC)** so that after the initial bulk load, ongoing data changes are synchronized.
- After the migration catches up, cut over application traffic to the new instance.

### 3. Swap endpoints / cut over to the new instance

1. Pause writes (or set the old instance to read-only) to avoid data drift during cutover.  
2. If using DMS/replication, ensure all changes are applied to the new instance and replication lag is minimal.  
3. Update your application configuration or DNS endpoints to point to the new instance’s endpoint.  
4. Verify operations against the new instance.  
5. Decommission (delete) the old instance if no longer needed (after backup retention as needed).


## Verification

- In the **RDS Console → Databases**, select the new instance and verify its **Master username** is your intended custom name (not the default).  
- Check that all databases, tables, views, stored procedures, and data have been migrated and are accessible.  
- Run your detection script again; it should now report the instance as **not using default master username**.  
- Perform end-to-end application tests (reads, writes, transactions) against the new instance to confirm correct behavior.
