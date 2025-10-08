# AWS / EC2 / EBS Backup Enabled

## Quick Info

| | |
|-|-|
| **Plugin Title** | EBS Backup Enabled |
| **Cloud** | AWS |
| **Category** | EC2 |
| **Description** | Checks whether EBS Backup is enabled |
| **More Info** | EBS volumes should have backups in the form of snapshots |
| **AWS Link** | https://docs.aws.amazon.com/prescriptive-guidance/latest/backup-recovery/new-ebs-volume-backups.html |
| **Recommended Action** | Ensure that each EBS volumes contain at least a backup in the form of a snapshot. |

### Introduction

EBS volumes are critical components of many AWS workloads, storing essential data for EC2 instances and applications. Regular backups, in the form of EBS snapshots, are crucial for data protection, disaster recovery, and business continuity. Snapshots provide a point-in-time copy of your EBS volumes, allowing you to restore data in case of accidental deletion, data corruption, or other unforeseen events. Without proper backup strategies, there is a risk of data loss and extended recovery times.

This guide outlines the steps to ensure that your EBS volumes are regularly backed up using snapshot creation.

### Detailed Remediation Steps

For ad-hoc or individual volume backups, you can create snapshots from the EC2 console by following the steps below.

1.  **Navigate to Volumes:**
    *   Open the Amazon EC2 console at [https://console.aws.amazon.com/ec2/](https://console.aws.amazon.com/ec2/).
    *   In the navigation pane, under **Elastic Block Store**, choose **Volumes**.
2.  **Select Volume:**
    *   Select the EBS volume you wish to back up.
3.  **Create Snapshot:**
    *   Choose **Actions**, then select **Create Snapshot**.
4.  **Configure Snapshot:**
    *   **Description:** Enter a meaningful description for the snapshot (e.g., "Backup before application upgrade").
    *   **Tags:** Add tags to the snapshot for better organization and management. A `Name` tag is highly recommended.
5.  **Create:**
    *   Choose **Create Snapshot**.

### Verification

To verify that your EBS volumes are being backed up, you can check the AWS console or use the AWS CLI.

1.  **AWS Console Verification:**
    *   Open the Amazon EC2 console at [https://console.aws.amazon.com/ec2/](https://console.aws.amazon.com/ec2/).
    *   In the navigation pane, under **Elastic Block Store**, choose **Snapshots**.
    *   Review the list of snapshots. Look for snapshots associated with your EBS volumes, checking their `Volume ID`, `Description`, and `Tags` to confirm they are the intended backups.

    
