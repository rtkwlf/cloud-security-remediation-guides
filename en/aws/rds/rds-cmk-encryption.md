[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)

# AWS / RDS / RDS CMK Encryption

## Quick Info

| | |
|-|-|
| **Plugin Title** | RDS CMK Encryption |
| **Cloud** | AWS |
| **Category** | RDS |
| **Description** | Ensures RDS instances are encrypted with KMS Customer Master Keys(CMKs). |
| **More Info** | RDS instances should be encrypted with Customer Master Keys in order to have full control over data encryption and decryption. |
| **AWS Link** | https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.Encryption.html |
| **Recommended Action** | RDS does not currently allow modifications to encryption after the instance has been launched, so a new instance will need to be created with KMS CMK encryption enabled. |

## Introduction

Amazon RDS supports encryption at rest via AWS KMS. Encryption **cannot** be enabled on an existing instance after launch. To encrypt, create a new instance by restoring from an encrypted snapshot using a Customer Managed Key (CMK).


## Detailed Remediation Steps

### 1. Create a manual snapshot of the existing RDS instance

- Open the **Amazon RDS Console**.  
- Select the RDS instance.  
- Choose **Actions > Take snapshot**.  
- Provide a snapshot name and confirm.  
- Wait for the snapshot status to become **Available**.

### 2. Copy the snapshot with encryption enabled

- Go to **Snapshots** in the RDS Console.  
- Select the snapshot created earlier.  
- Click **Actions > Copy Snapshot**.  
- Enable **Encryption**.  
- Select your **Customer Managed KMS Key**.  
- Name the new snapshot and confirm.  
- Wait for the encrypted snapshot to become **Available**.

### 3. Restore a new RDS instance from the encrypted snapshot

- Select the encrypted snapshot in **Snapshots**.  
- Choose **Actions > Restore Snapshot**.  
- Configure instance settings as needed (instance class, subnet group, etc.).  
- Confirm **Storage encryption** is enabled with your selected KMS key.  
- Restore the new instance.

### 4. Update your applications to use the new instance

- Change your applications’ connection strings or DNS endpoints to point to the new encrypted instance.  
- Perform any necessary data synchronization or cutover to ensure continuity.

### 5. Delete the old unencrypted RDS instance

- After confirming everything works on the new instance:  
- Select the old instance in **Databases**.  
- Choose **Actions > Delete**.  
- Decide whether to keep a final snapshot or not, then confirm deletion.


## Verification

- In the RDS Console, select the new instance.  
- Verify **Storage Encryption** is **Enabled**.  
- Check the **KMS key ID** matches your Customer Managed Key.  
- Run your detection script; it should show compliance now.  
- Test application connectivity and data integrity.
