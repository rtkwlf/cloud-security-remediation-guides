# AWS / RDS / RDS DocumentDB Minor Version Upgrade

## Quick Info

| | |
|-|-|
| **Plugin Title** | RDS DocumentDB Minor Version Upgrade |
| **Cloud** | AWS |
| **Category** | RDS |
| **Description** | Ensures Auto Minor Version Upgrade is enabled on RDS and DocumentDB databases |
| **More Info** | RDS supports automatically upgrading the minor version of the database, which should be enabled to ensure security fixes are quickly deployed. |
| **AWS Link** | https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.Upgrading.html#USER_UpgradeDBInstance.Upgrading.AutoMinorVersionUpgrades |
| **Recommended Action** | Enable automatic minor version upgrades on RDS and DocumentDB databases |


## Introduction

Automatic minor version upgrades allow Amazon RDS and DocumentDB to apply minor engine version updates automatically during your maintenance window. This helps keep your databases secure with the latest patches and improvements, without manual intervention.


## Detailed Remediation Steps

### Enable Auto Minor Version Upgrades for Amazon RDS

1. Sign in to the **AWS Management Console** and open the [Amazon RDS Console](https://console.aws.amazon.com/rds/).
2. In the left navigation pane, click **Databases**.
3. From the list, click on the DB instance you want to modify.
4. At the top right, click **Actions**, then select **Modify**.
5. Scroll down to the **Database options** section.
6. Find the option labeled **Auto minor version upgrade**.
7. Check the box next to **Auto minor version upgrade** to enable it.
8. Scroll to the bottom and click **Continue**.
9. On the next page, review the summary of changes.
10. Choose when to apply the changes:  
    - Select **Apply immediately** to implement changes right away (may cause a brief downtime).  
    - Or choose to apply changes during the next maintenance window for zero downtime.
11. Click **Modify DB instance** to save your changes.


### Enable Auto Minor Version Upgrades for Amazon DocumentDB

1. Open the [Amazon DocumentDB Console](https://console.aws.amazon.com/docdb/).
2. In the left navigation pane, select **Clusters**.
3. Click on the cluster where you want to enable auto minor version upgrades.
4. Choose **Modify** from the **Actions** dropdown.
5. Scroll to the **Maintenance** section.
6. Locate the **Auto minor version upgrade** option.
7. Select the checkbox to enable this feature.
8. Scroll down and click **Modify Cluster**.
9. Choose whether to apply changes immediately or during the next maintenance window.


## Verification

1. Go to the **Amazon RDS Console**, click **Databases**, and select your DB instance.
2. In the **Configuration** section, confirm that **Auto minor version upgrade** is set to **Enabled**.
3. For DocumentDB, open the **Amazon DocumentDB Console**, click **Clusters**, and select your cluster.
4. Verify that **Auto minor version upgrade** is enabled under the **Maintenance** section.
5. Run your detection script again. Instances with auto minor upgrades enabled should be reported as compliant.
6. Test your applications to confirm they are functioning correctly after any maintenance or upgrade.

