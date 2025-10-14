[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)

# AWS / RDS / RDS Public Subnet

## Quick Info

| | |
|-|-|
| **Plugin Title** | RDS Public Subnet |
| **Cloud** | AWS |
| **Category** | RDS |
| **Description** | Ensures RDS database instances are not deployed in public subnet. |
| **More Info** | RDS instances should not be deployed in public subnets to prevent direct exposure to the internet and reduce the risk of unauthorized access. |
| **AWS Link** | https://docs.aws.amazon.com/config/latest/developerguide/rds-instance-public-access-check.html |
| **Recommended Action** | Replace the subnet groups of rds instance with the private subnets. |


## Introduction

RDS instances placed in public subnets are at higher risk of exposure to the internet. To improve security, instances should reside in private subnets (i.e. subnets without routes to an Internet Gateway). In AWS terms, this means using a DB subnet group composed of **private subnets only**.  

Because RDS does **not provide a direct “move to private subnet” button**, the remediation typically involves updating the DB subnet group to include only private subnets (and exclude public ones), then modifying the DB instance to use that subnet group. In some cases, you may need to work around Multi‑AZ or failover mechanisms.

This guide walks you through the console steps to replace a subnet group with private subnets and apply it to the RDS instance.


## Detailed Remediation Steps

### 1. Identify private subnets and create a private-only DB subnet group (if not existing)

- In the AWS Management Console, go to **VPC → Subnets** and identify subnet IDs that do **not** have routes to an Internet Gateway (i.e. private subnets).  
- In the RDS Console, navigate to **Subnet groups**.  
- Click **Create DB Subnet Group** (or clone an existing one).  
  - Select your VPC.  
  - Add only **private subnets** (at least one per AZ, spanning two or more AZs).  
  - Provide a name and description (e.g. `private-only-subnet-group`).  
  - Save the new DB subnet group.

### 2. Edit the existing DB subnet group associated with the instance (optional)

- In **RDS → Subnet groups**, select the subnet group currently used by the instance.  
- Click **Edit**.  
- Remove public subnets from the membership.  
- Add private subnets (if not already included).  
- Save changes.  
  > *Note:* You may not be able to remove subnets that are currently in use by the DB instance.

### 3. Modify the DB instance to use the private-only DB subnet group

- In the **RDS Console → Databases**, pick the instance you wish to change.  
- Click **Actions → Modify**.  
- Under **Network & Security** (or connectivity settings), change the **DB Subnet Group** to your private-only subnet group name.  
- Also, uncheck **Publicly accessible** if that option exists.  
- Click **Continue**.  
- On the summary page, choose whether to **Apply immediately** (causing downtime) or defer to the **next maintenance window**.  
- Confirm by clicking **Modify DB instance**.

### 4. Handle Multi‑AZ and failover (if applicable)

- If the instance is Multi‑AZ, AWS may perform the change on the standby first and then fail over; the primary may move into a private subnet accordingly.  
- After the change is done, verify that both primary and standby are in private subnets.


## Verification

1. In the RDS Console, go to **Databases** and select the instance.  
   - Under **Connectivity & security**, verify **DB Subnet Group** is your private-only group.  
   - Confirm **Publicly accessible** is set to **No** (if applicable).  
2. In **Subnet groups**, inspect the subnet group used by your instance and verify **all** its subnets are private (i.e. no subnet has a route to an Internet Gateway).  
3. Re-run your detection script: the instance should no longer be flagged as in a public subnet.  
4. (Optional) Test that inbound traffic from the internet is blocked (i.e. the instance endpoint is inaccessible from the public internet).
