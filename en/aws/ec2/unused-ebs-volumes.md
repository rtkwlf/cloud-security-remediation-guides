# AWS / EC2 / Unused EBS Volumes

## Quick Info

| | |
|-|-|
| **Plugin Title** | Unused EBS Volumes |
| **Cloud** | AWS |
| **Category** | EC2 |
| **Description** | Ensures EBS volumes are in use and attached to EC2 instances |
| **More Info** | EBS volumes should be deleted if the parent instance has been deleted to prevent accidental exposure of data. |
| **AWS Link** | https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-deleting-volume.html |
| **Recommended Action** | Delete the unassociated EBS volume. |

### Introduction

Amazon EBS volumes are block-level storage devices that can be attached to EC2 instances. When an EC2 instance is terminated, its associated EBS volumes might persist depending on their deletion on termination attribute. Volumes that are no longer attached to any instance are considered "unassociated" or "unused." Deleting these volumes helps reduce costs and ensures that no stale data remains accessible. Once an EBS volume is deleted, its data is permanently gone and the volume cannot be re-attached to any instance.

This guide provides detailed steps to remediate the finding of unassociated Amazon Elastic Block Store (EBS) volumes, as detected by the provided script. Unassociated EBS volumes are not attached to any EC2 instance and can incur unnecessary costs and potentially expose data if not properly managed.

### Prerequisites

Before proceeding with the deletion of an EBS volume, ensure the following:

*   **Volume State**: The EBS volume must be in an `available` state. You cannot delete a volume if it is currently attached to an EC2 instance. If the volume is attached, you must first detach it. Refer to the AWS documentation on [Detaching an Amazon EBS volume from an Amazon EC2 instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-detaching-volume.html) if necessary.
*   **Data Backup (Optional but Recommended)**: If there is any possibility that the data on the volume might be needed in the future, create a snapshot of the volume before deletion. This allows you to re-create the volume from the snapshot later if required.

### Remediation Steps

Follow these steps to delete an unassociated EBS volume.

#### Using the AWS Management Console

1.  Open the Amazon EC2 console at [https://console.aws.amazon.com/ec2/](https://console.aws.amazon.com/ec2/).
2.  In the navigation pane, choose **Volumes** under **Elastic Block Store**.
3.  Select the unassociated volume you wish to delete.
4.  Verify that the volume is in the **Available** state.
5.  Choose **Actions**, then select **Delete volume**.
    *   *Note*: If the **Delete volume** option is disabled, the volume is still attached to an instance and cannot be deleted.
6.  When prompted for confirmation, type `delete` in the confirmation box, and then choose **Delete**.
    
