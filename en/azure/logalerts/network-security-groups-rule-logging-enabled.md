[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)

# AZURE / Log Alerts / Network Security Groups Rule Logging Enabled

## Quick Info

| | |
|-|-|
| **Plugin Title** | Network Security Groups Rule Logging Enabled |
| **Cloud** | AZURE |
| **Category** | Log Alerts |
| **Description** | Ensures Activity Log alerts for the create or update and delete Network Security Group rule events are enabled |
| **More Info** | Monitoring for create or update and delete Network Security Group rule events gives insight into network access changes and may reduce the time it takes to detect suspicious activity. |
| **AZURE Link** | https://docs.microsoft.com/en-us/azure/azure-monitor/platform/activity-log-alerts |
| **Recommended Action** | Add a new log alert to the Alerts service that monitors for Network Security Group rule create or update and delete events. |

## Detailed Remediation Steps

1. Log into the Microsoft Azure Management Console.
2. In the top search bar, search for "Alerts" and select "Alerts" from the results.
3. Click on "Manage alert rules" from the top panel.
4. Click "New alert rule" to create a new rule.
5. Under "Resources", click "Select", search for "Network Security Groups", and select the appropriate NSG Resource.
6. Under "Condition", click "Add", search for "Administrative Category", and select "Create or Update Network Security Group Rule" then "Delete Network Security Group Rule". Click "Done".
7. Under "Actions", select an existing "Action group" or click "Create action group" to configure email or other notifications.
8. Provide an Alert rule name, enable "Yes" under "Enable rule upon creation", and click "Create alert rule" to finalize.




