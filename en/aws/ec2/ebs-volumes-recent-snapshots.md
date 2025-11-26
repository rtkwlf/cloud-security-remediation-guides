# AWS / EC2 / EBS Volumes Recent Snapshots

## Quick Info

| | |
|-|-|
| **Plugin Title** | EBS Volumes Recent Snapshots |
| **Cloud** | AWS |
| **Category** | EC2 |
| **Description** | Ensures that EBS volume has had a snapshot within the last 7 days |
| **More Info** | EBS volumes without recent snapshots may be at risk of data loss or recovery issues. |
| **AWS Link** | https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSSnapshots.html |
| **Recommended Action** | Create a new snapshot for EBS volume weekly. |

### Introduction

This guide outlines the steps to ensure your Amazon Elastic Block Store (EBS) volumes have recent snapshots, specifically by implementing a weekly snapshot schedule. Regular snapshots are crucial for data protection, disaster recovery, and compliance, safeguarding against data loss and enabling efficient recovery. We will leverage Amazon Data Lifecycle Manager (DLM) to automate the creation, retention, and deletion of EBS snapshots, providing a robust and cost-effective backup solution.

### Prerequisites

Before you begin, ensure you have the following:

*   **AWS Account and Permissions**: You need an AWS account with appropriate permissions to create and manage Amazon Data Lifecycle Manager policies, EBS volumes, and snapshots. Specifically, the IAM role used by DLM must have permissions to:
    *   `ec2:CreateSnapshot`
    *   `ec2:DeleteSnapshot`
    *   `ec2:CreateTags`
    *   `ec2:DescribeVolumes`
    *   `ec2:DescribeSnapshots`
    *   `ec2:DescribeTags`
    *   `ec2:ModifySnapshotAttribute` (if sharing snapshots)
    *   `ec2:CopySnapshot` (if copying snapshots across regions/accounts)
*   **EBS Volumes**: Identify the EBS volumes for which you want to automate snapshot creation. It is recommended to tag these volumes consistently to easily target them with DLM policies.

### Detailed Remediation Steps

To automate weekly snapshots for your EBS volumes, you will create a custom Amazon Data Lifecycle Manager policy.

1.  **Navigate to Amazon Data Lifecycle Manager**:
    *   Open the Amazon EC2 console at [https://console.aws.amazon.com/ec2/](https://console.aws.amazon.com/ec2/).
    *   In the navigation pane, under **Elastic Block Store**, choose **Lifecycle Manager**.

2.  **Create Lifecycle Policy**:
    *   Choose **Create new lifecycle policy**.
    *   Choose **Custom policy**
    *   For **Policy type**, select **EBS Snapshot Policy**.
    *   Click **Next step**

3.  **Configure Policy Details**:
    *   For **Target resource type**, select **Volume**.
    *   **Target resource tags**:
        *   Choose specific tags of the EBS volumes you want to include in this policy. For example, if your critical volumes are tagged with `Environment:Production` and `Backup:Enabled`, add these tags. DLM will only create snapshots for volumes that match *all* specified tags.
    *   **Description**: Provide a meaningful description for your policy (e.g., "Weekly EBS Snapshot Policy for Critical Volumes").
    *   **IAM role**: Choose an existing IAM role with the necessary permissions (as outlined in the Prerequisites) or create a new one.
    *   **Tags**: (Optional) Add tags to the DLM policy itself for organization and cost allocation.
    *   **Policy status**: Select **Enable** to activate the policy immediately.

4.  **Configure Schedule**:
    *   Choose **Add schedule**.
    *   **Schedule name**: Give your schedule a name (e.g., "WeeklySnapshotSchedule").
    *   **Frequency**: Select **Weekly**.
    *   **Start time**: Specify the time (in UTC) when you want the snapshots to be created. Choose a time outside of peak operational hours.
    *   **Retention type**: Select **Count**.
    *   **Keep**: Enter `4` to retain the last four weekly snapshots (approximately one month's worth). Adjust this value based on your recovery point objective (RPO) and compliance requirements.
    *   **Tagging**:
        *   **Copy tags from source**: Select this option to copy tags from the source EBS volume to the snapshot. This is highly recommended for easier identification and management of snapshots.
        *   **Add tags to snapshots**: (Optional) Add additional tags to the snapshots created by this policy (e.g., `ManagedBy:DLM`, `Retention:4Weeks`).
    *   **Fast Snapshot Restore**: (Optional) If you require very fast restoration of volumes from these snapshots, you can enable Fast Snapshot Restore (FSR) and specify the Availability Zones. Be aware of the additional costs associated with FSR.
    *   **Cross-Region copy**: (Optional) If you need to copy snapshots to another AWS Region for disaster recovery, enable this option and specify the destination Region and retention settings for the copied snapshots.
    *   **Cross-account sharing**: (Optional) If you need to share snapshots with another AWS account, enable this option and specify the target account ID.

5.  **Review and Create Policy**:
    *   Review all the policy settings.
    *   Choose **Create policy**.

Once the policy is created and enabled, Amazon Data Lifecycle Manager will automatically create snapshots of your tagged EBS volumes according to the defined schedule.

### Verification

To verify that your DLM policy is working as expected and creating weekly snapshots:

1.  **Monitor Policy Status**:
    *   In the EC2 console, navigate to **Lifecycle Manager**.
    *   Check the **State** of your newly created policy. It should be `Enabled`.
    *   Review the **Last run** and **Next run** times to confirm the schedule is active.

2.  **Check Snapshot Creation**:
    *   In the EC2 console, navigate to **Snapshots**.
    *   Filter the snapshots by the tags you applied to your EBS volumes or the tags added by the DLM policy (e.g., `ManagedBy:DLM`).
    *   Verify that new snapshots are being created on a weekly basis, matching the schedule you configured.
    *   Check the **Creation time** of the snapshots to ensure they align with your policy's start time.

### Next Steps

*   **Review Retention Policies**: Regularly review your snapshot retention settings to ensure they align with your organizational policies, compliance requirements, and cost optimization goals. Adjust the `Retain` count in your DLM policy as needed.
*   **Test Restoration Process**: Periodically test the restoration of an EBS volume from a snapshot to ensure your backup strategy is effective and you can recover data successfully.
*   **Consider Multi-Volume Snapshots**: If you have EC2 instances with multiple EBS volumes that need to be backed up consistently (e.g., for a database server), consider creating a DLM policy for EBS-backed AMIs or using multi-volume snapshots to ensure application consistency.
*   **Cross-Region/Cross-Account Copies**: If your disaster recovery strategy requires it, configure cross-Region or cross-account snapshot copies within your DLM policy for enhanced data resilience.
