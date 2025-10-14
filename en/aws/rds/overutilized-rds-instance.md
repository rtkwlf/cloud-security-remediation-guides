# AWS / RDS / RDS CPU Alarm Threshold Exceeded

## Quick Info

| | |
|-|-|
| **Plugin Title** | RDS CPU Alarm Threshold Exceeded |
| **Cloud** | AWS |
| **Category** | RDS |
| **Description** | Ensure RDS instances do not exceed the alarm threshold for CPU utilization. |
| **More Info** | High CPU usage may suggest that the databases on these servers lack sufficient hardware resources to operate at their best. Enhancing the performance of overburdened RDS instances by upgrading them can directly enhance the well-being and performance of the databases. |
| **AWS Link** | https://docs.aws.amazon.com/prescriptive-guidance/latest/amazon-rds-monitoring-alerting/db-instance-cloudwatch-metrics.html |
| **Recommended Action** | Upgrade (upsize) the overused RDS database instances. |


## Introduction

When an RDS instance consistently hits high CPU usage or performance bottlenecks, it indicates that the current instance class (compute / memory) is insufficient. Upgrading (upsizing) the instance to a larger class offers more CPU, memory, and networking throughput, alleviating pressure on the database workload.

Amazon RDS supports modifying existing DB instances to change the `DBInstanceClass`. Such modifications may cause downtime (a reboot).
The API `ModifyDBInstance` allows changes to the `DBInstanceClass` field.

This guide outlines how to upgrade (upsizes) an RDS instance via the AWS Management Console and how to verify the change.


## Detailed Remediation Steps

### 1. Open the Amazon RDS Console and locate the target DB instance  
- Sign in to the AWS Management Console.  
- Navigate to **Amazon RDS**.  
- In the left pane, click **Databases**.  
- Select the instance that is over‑utilized and needs upgrading.

### 2. Choose **Modify** on the instance  
- With your DB instance selected, click **Actions → Modify**.  
- In the modify settings, locate the **DB instance class** dropdown.  
- Select a larger, more capable instance class (e.g. move from `db.m5.large` → `db.m5.xlarge` or `db.r5.large` → `db.r5.xlarge`) that fits your performance needs.

### 3. Review other instance settings if needed  
- If you have parameter groups, security groups, storage, or other configuration adjustments, review them.  
- Ensure that the chosen new class is compatible with your current engine version and features.

### 4. Decide when to apply the change  
- Click **Continue** to proceed to summary.  
- Choose whether to **Apply immediately** (which triggers downtime immediately) or during the **next maintenance window** (to schedule the upgrade at a less disruptive time).  
- Confirm your changes by clicking **Modify DB instance**.

### 5. Monitor upgrade and wait for completion  
- The DB instance will enter a **modifying** state.  
- Wait until the status becomes **available**—the new instance class is active.  
- For Multi-AZ setups, AWS may apply the change to the standby first and then fail over to minimize downtime.


## Verification

- In the RDS Console under **Databases**, select your upgraded instance.  
- In the **Details / Configuration** section, verify that the **DB instance class** matches the new, larger class you selected.  
- Confirm the instance status is **Available** (i.e. upgrade is complete).  
- Re-run your detection script—the instance should no longer be flagged as overutilized (if CPU usage drops accordingly).  
- Perform application tests (reads, writes, performance benchmarks) to ensure the new class meets workload demand.
