[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)

# AWS / RDS / RDS Instance Generation

## Quick Info

| | |
|-|-|
| **Plugin Title** | RDS Instance Generation |
| **Cloud** | AWS |
| **Category** | RDS |
| **Description** | Ensures that AWS RDS instance is not using older generation of EC2 |
| **More Info** | Amazon RDS instances running on older generation EC2 instances may not have access to the latest hardware capabilities and performance improvements. It is recommended to upgrade the RDS instance to its latest generation for optimal performance and security. |
| **AWS Link** | https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.DBInstanceClass.html |
| **Recommended Action** | Upgrade the instance to its latest generation. |


## Introduction

Using older-generation DB instance classes in Amazon RDS can limit performance, security, and access to newer features. AWS encourages upgrading to the latest generation of instance classes to benefit from more efficient hardware, newer virtualization (Nitro), improved networking, and future support.

Amazon RDS allows you to **modify** your DB instance to a new instance class (which may correspond to a newer generation), though such changes can cause downtime.

This guide describes how to perform this upgrade via the AWS Management Console and how to verify the change.


## Detailed Remediation Steps

### 1. Open the Amazon RDS Console and locate your DB instance  
- Log in to AWS Management Console.  
- Go to **Amazon RDS**.  
- In the left pane, click **Databases**.  
- Select the DB instance you want to upgrade.

### 2. Initiate modification of the DB instance  
- With the instance selected, click **Actions → Modify**.  
- In the **DB instance class** setting (under configuration or settings), choose a newer generation instance class (e.g. moving from `db.m4` → `db.m5`, or `db.r4` → `db.r5`, or from `db.t2` → `db.t3`) that is supported in your environment and engine.
- Review other settings if needed (e.g. parameter groups, storage, maintenance).  

### 3. Choose timing for applying the change  
- Click **Continue**.  
- On the summary page, select whether to apply the change **Immediately** (which triggers outage) or during the **Next maintenance window** (to minimize disruption).
- Confirm the modification by clicking **Modify DB instance**.

### 4. Monitor the change and wait until it completes  
- The DB instance status will change (e.g. “modifying”).  
- Wait until the status becomes **Available**—this means the new instance class is active.  
- For Multi-AZ instances, AWS will perform the upgrade on standby first and then fail over (if applicable). 


## Verification

- In the **RDS Console → Databases**, select the instance.  
- In the **Configuration / Details** section, check the **DB instance class**; it should reflect the new, latest-generation class you selected.  
- Confirm the instance status is **Available** (i.e. modification complete).  
- Re-run your detection plugin script—instances using older-generation classes (e.g. those specified in the script) should now register as compliant.  
- Validate application functionality (reads, writes) to ensure the upgrade did not break anything.
