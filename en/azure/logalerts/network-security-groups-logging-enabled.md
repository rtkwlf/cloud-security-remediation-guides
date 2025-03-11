[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)

# AZURE / Log Alerts / Network Security Groups Logging Enabled

## Quick Info

| | |
|-|-|
| **Plugin Title** | Network Security Groups Logging Enabled |
| **Cloud** | AZURE |
| **Category** | Log Alerts |
| **Description** | Ensures Activity Log alerts for the create or update and delete Network Security Group events are enabled |
| **More Info** | Monitoring for create or update and delete Network Security Group events gives insight into network access changes and may reduce the time it takes to detect suspicious activity. |
| **AZURE Link** | https://docs.microsoft.com/en-us/azure/azure-monitor/platform/activity-log-alerts |
| **Recommended Action** | Add a new log alert to the Alerts service that monitors for Network Security Group create or update and delete events. |

## Detailed Remediation Steps

1. Log into the Microsoft Azure Management Console.
2. Select the "Search resources, services, and docs" option at the top and search for Alerts.
3. On the "Alerts" page, click on "Manage alert rules" at the top panel.
4. On the "Rules" page, scroll down and check the "Target Resource Type" to verify if there are any existing rules for "Network Security Groups (NSG)" create or update and delete events. If no alerts are configured, then Activity Log Alerts for NSG events are not enabled.
5. Repeat steps 2 - 4 to check other Azure accounts.
6. Navigate to "Alerts" and click on "New alert rule" at the top.
7. On the "Create rule" page, click on "Select" under "Resources", search for "Network Security Groups" under "Filter by resource type" and select the appropriate NSG Resource.
8. On the "Create rule" page, click on "Add" under "Condition", search for "Administrative Category", and select "Create or Update Network Security Group". Then, "Delete Network Security Group" Click "Done" at the bottom of the tab.
9. Under "Actions", select an existing "Action group" or click "Create action group" accordingly.
10. Enter the "Alert rule name" and "Description" under "Alert Details" and enable "Yes" under "Enable rule upon creation" to activate the alert immediately. Click "Create alert rule" at the bottom to finalize.
11. Repeat steps 6 - 10 to add additional log alerts for monitoring NSG create or update and delete events across different accounts.
