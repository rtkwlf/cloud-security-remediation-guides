[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)

# AWS / RDS / RDS Snapshot Publicly Accessible

## Quick Info

| | |
|-|-|
| **Plugin Title** | RDS Snapshot Publicly Accessible |
| **Cloud** | AWS |
| **Category** | RDS |
| **Description** | Ensure that Amazon RDS database snapshots are not publicly exposed. |
| **More Info** | If an RDS snapshot is exposed to the public, any AWS account can copy the snapshot and create a new database instance from it. It is a best practice to ensure RDS snapshots are not exposed to the public to avoid any accidental leak of sensitive information. |
| **AWS Link** | https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ShareSnapshot.html |
| **Recommended Action** | Ensure Amazon RDS database snapshot is not publicly accessible and available for any AWS account to copy or restore it. |


## Introduction

Publicly shared RDS snapshots allow **any AWS account** to copy or restore them, which can lead to unintended data exposure. According to AWS, you can share an unencrypted manual snapshot as **public**, which gives all accounts permission to copy or restore that snapshot.

To prevent information leakage, you must ensure that manual snapshots are marked **private** (i.e. not publicly visible) and, where necessary, revoke “public” access. AWS also provides mechanisms to stop sharing snapshots with accounts or make them private.

This guide describes how to change snapshot visibility (via the console) and prevent public accessibility.


## Detailed Remediation Steps

### 1. Open the Amazon RDS Console and go to Snapshots  
- Sign in to the AWS Management Console.  
- Navigate to **Amazon RDS** service.  
- In the left menu, choose **Snapshots** (Manual snapshots view).

### 2. Identify snapshots that are set to public visibility  
- In the Snapshots page, filter or look for snapshots with **Visibility = Public**.  
- You may also view the “Public” tab to see externally exposed snapshots.

### 3. Change snapshot visibility to Private (remove public access)  
- Select a snapshot that is currently public.  
- Click **Actions → Share snapshot**.  
- On the *Manage Snapshot Permissions* page, set **DB Snapshot Visibility** to **Private**. (This removes the “public” flag.)
- If there are specific AWS account IDs granted access, remove them from the list.  
- Save changes.

### 4. Repeat for all public snapshots  
- For each manual snapshot in the public view, perform the same “Share snapshot → set to Private” operation.  
- Ensure no snapshot remains marked as **public**.


## Verification

1. In the Amazon RDS Console, return to **Snapshots** and switch to the **Public** tab.  
   - Confirm no snapshots owned by your account appear there.  
2. For each snapshot, select it and open **Actions → Share snapshot**, and ensure **DB Snapshot Visibility** is set to **Private**.  
3. Re-run your detection script. Snapshots that were previously flagged as publicly exposed should now show as non‑public (compliant).  
4. Optionally, attempt (via another AWS account) to copy or restore one of your snapshots (that was converted to private) — it should be denied.
