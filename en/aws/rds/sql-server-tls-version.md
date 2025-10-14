[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)

# AWS / RDS / SQL Server TLS Version

## Quick Info

| | |
|-|-|
| **Plugin Title** | SQL Server TLS Version |
| **Cloud** | AWS |
| **Category** | RDS |
| **Description** | Ensures RDS SQL Servers do not allow outdated TLS certificate versions |
| **More Info** | TLS 1.2 or higher should be used for all TLS connections to RDS. A parameter group can be used to enforce this connection type. |
| **AWS Link** | https://aws.amazon.com/about-aws/whats-new/2020/07/amazon-rds-for-sql-server-supports-disabling-old-versions-of-tls-and-ciphers/ |
| **Recommended Action** | Create a parameter group that contains the TLS version restriction and limit access to TLS 1.2 or higher |


## Introduction

Amazon RDS for SQL Server supports disabling older TLS versions (TLS 1.0 and TLS 1.1) and ciphers via DB parameter settings. :contentReference[oaicite:0]{index=0}  
By creating a custom DB parameter group and setting `rds.tls10 = disabled` and `rds.tls11 = disabled`, you can restrict client connections so that only TLS 1.2 (and higher) is allowed. :contentReference[oaicite:1]{index=1}  
Note: The `rds.tls12` parameter cannot be modified (it is fixed). :contentReference[oaicite:2]{index=2}  

This guide walks you through creating or updating a parameter group and applying it to your RDS SQL Server instance so that only TLS 1.2+ is permitted.


## Detailed Remediation Steps

### 1. Create a custom DB parameter group (if not already present)

- In the **AWS Management Console**, go to **Amazon RDS → Parameter groups**.  
- Click **Create parameter group**.  
- For **Parameter group family**, choose the SQL Server engine version family matching your instance (e.g. `sqlserver-se-13.0`, etc.). :contentReference[oaicite:3]{index=3}  
- Provide a **Group name** (e.g. `sqlserver-tls-restrict`) and a **Description** (e.g. “Enforce TLS 1.2+ only”).  
- Save / create the parameter group.

### 2. Modify parameters to disable TLS 1.0 and TLS 1.1

- In the **Parameter groups** console, select your custom group.  
- In its **Parameters** tab, filter by prefix `rds.` (or search for `tls`).  
- Click **Edit parameters**.  
- Locate the parameter `rds.tls10` and set its value to **disabled**. :contentReference[oaicite:4]{index=4}  
- Locate `rds.tls11` and set it to **disabled**. :contentReference[oaicite:5]{index=5}  
- Leave `rds.tls12` untouched (it cannot be modified). :contentReference[oaicite:6]{index=6}  
- (Optional) Also disable insecure ciphers such as `rds.rc4`, `rds.curve25519`, `rds.3des168` etc., according to your security policy. :contentReference[oaicite:7]{index=7}  
- After making changes, click **Save changes**.

### 3. Apply the custom parameter group to your SQL Server instance

- Navigate to **Amazon RDS → Databases**.  
- Select the SQL Server DB instance you wish to enforce TLS restrictions on.  
- Click **Actions → Modify**.  
- In **Database options** (or a similar section), choose the custom parameter group you just modified (e.g. `sqlserver-tls-restrict`). :contentReference[oaicite:8]{index=8}  
- Scroll to bottom and click **Continue**.  
- On the confirmation page, review settings.  
- Because these TLS parameters are **static**, a **reboot of the DB instance** is required for changes to take effect. :contentReference[oaicite:9]{index=9}  
- Choose to apply immediately (causing downtime) or at the next maintenance window.  
- Click **Modify DB instance** to apply.


## Verification

- In the **RDS Console → Databases**, select the instance. Under its **Configuration** or **Parameter group** section, confirm the instance is associated with the custom parameter group you created.  
- Open **Parameter groups**, go to your custom group, view **Parameters**, and verify that:  
  - `rds.tls10 = disabled`  
  - `rds.tls11 = disabled`  
  - `rds.tls12` remains as default (cannot be modified)  
- Re-run your detection script. It should now report compliance: i.e. “DB parameter group … uses TLS 1.2” (or similar) rather than “does not require TLS 1.2”.  
- Test connecting to your SQL Server instance using different TLS versions: attempts to connect with TLS 1.0 or TLS 1.1 should fail, and connections via TLS 1.2+ should succeed.
