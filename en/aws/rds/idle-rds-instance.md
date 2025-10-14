[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)

# AWS / RDS / RDS Idle Instance Status

## Quick Info

| | |
|-|-|
| **Plugin Title** | RDS Idle Instance Status |
| **Cloud** | AWS |
| **Category** | RDS |
| **Description** | Ensure there are no RDS instances with CPU utilization below all of the defined thresholds within last 7 days. |
| **More Info** | HIdle Amazon RDS instance is a prime candidate for reducing monthly AWS expenses and preventing unnecessary usage charges from accumulating. |
| **AWS Link** | https://docs.aws.amazon.com/prescriptive-guidance/latest/amazon-rds-monitoring-alerting/db-instance-cloudwatch-metrics.html |
| **Recommended Action** | Identify and remove idle RDS instance |


## Introduction

Amazon RDS instances can accumulate unnecessary cost if left running without actual workload. Idle RDS instances—those showing consistently low CPU utilization, read/write IOPS, and connections over a monitoring period (e.g., 7 days)—should be reviewed and considered for termination or downsizing.

Removing idle RDS instances helps optimize cloud resource usage and reduce monthly AWS costs. It is a recommended cost-optimization strategy, especially in dev/test environments or after project completion.


## Detailed Remediation Steps

### 1. Review CloudWatch Metrics to Confirm Idleness

- Go to the **Amazon CloudWatch Console**.
- Navigate to **Metrics > RDS > Per-Database Metrics**.
- Search and select the RDS instance in question.
- Check the following metrics over the **past 7 days**:
  - **CPUUtilization**
  - **ReadIOPS**
  - **WriteIOPS**
- Confirm that the instance consistently reports:
  - CPU utilization below 1%
  - Read IOPS below 20
  - Write IOPS below 20

> These are default thresholds defined in the detection logic. Adjust based on business context.

### 2. Backup the Database (Optional but Recommended)
- In the **Amazon RDS Console**, go to **Databases**, select the instance.
- Choose **Actions → Take snapshot**.
- Enter a snapshot name and click **Take snapshot**.
- Wait for the snapshot status to become **Available** before proceeding.

### 3. Identify Dependencies and Detach or Reconfigure Them
- Confirm the instance is not connected to any applications, monitoring agents, or data pipelines.
- If so, remove or reconfigure these to prevent errors post-deletion.

### 4. Delete the Idle RDS Instance
- In the **Amazon RDS Console**, navigate to **Databases**.
- Select the idle DB instance.
- Click **Actions → Delete**.
- On the confirmation screen:
  - If you created a manual snapshot earlier, uncheck the box for final snapshot (optional).
  - Confirm deletion by typing the DB instance identifier.
- Click **Delete**.

> **Important**: Deletion is irreversible unless a manual snapshot was taken.


## Verification

- In the **RDS Console**, confirm the instance no longer appears under **Databases**.
- Go to **Snapshots** to ensure the backup (if created) is present and **Available**.
- Review billing and resource usage in **AWS Cost Explorer** or **Billing Dashboard** to verify reduced cost impact.
- Re-run the detection script to ensure the idle instance no longer appears in the result set.
