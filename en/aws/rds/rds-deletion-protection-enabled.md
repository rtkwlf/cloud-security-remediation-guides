# AWS / RDS / RDS Deletion Protection Enabled

## Quick Info

| | |
|-|-|
| **Plugin Title** | RDS Deletion Protection Enabled |
| **Cloud** | AWS |
| **Category** | RDS |
| **Description** | Ensures deletion protection is enabled for RDS database instances. |
| **More Info** | Deletion protection prevents Amazon RDS instances from being deleted accidentally by any user. |
| **AWS Link** | https://aws.amazon.com/about-aws/whats-new/2018/09/amazon-rds-now-provides-database-deletion-protection/ |
| **Recommended Action** | Modify the RDS instances to enable deletion protection. |


## Introduction

**Deletion protection** is a security feature in Amazon RDS that prevents accidental or unauthorized deletion of database instances. When enabled, even users with permissions to delete the instance must first disable this setting before deletion is allowed. This control is especially important for production or critical workloads where data integrity and availability are paramount.

> ⚠️ This feature must be **manually enabled** after RDS instance creation unless it was set during provisioning.


## Detailed Remediation Steps

Follow the steps below to **enable deletion protection** on an existing Amazon RDS instance using the **AWS Management Console**:

### 1. Navigate to the RDS Console
- Go to the [Amazon RDS Console](https://console.aws.amazon.com/rds/)
- In the left navigation pane, select **Databases**
- Locate and click on the RDS instance you want to modify

### 2. Modify the RDS Instance
- With the DB instance selected, click the **Actions** dropdown
- Choose **Modify**

### 3. Enable Deletion Protection
- Scroll down to the **Additional configuration** section
- Locate the **Deletion protection** option
- Check the box labeled **"Enable deletion protection"**

### 4. Apply the Change
- At the bottom of the page, under **Scheduling of modifications**, select one of the following:
  - **Apply immediately** – applies the change right away (may cause downtime)
  - **Apply during the next scheduled maintenance window** – schedules the change later (no immediate downtime)
- Click **Continue**, then **Modify DB instance** to save the changes


## Verification

After completing the remediation, verify that deletion protection is successfully enabled:

1. In the RDS console, go back to the **Databases** section
2. Click on the name of the modified RDS instance
3. Under the **Configuration** tab, locate the field **Deletion protection**
4. Confirm that the value is set to **"Enabled"**
