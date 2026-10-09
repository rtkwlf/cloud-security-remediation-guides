# AZURE / Log Alerts / Security Solution Logging

## Quick Info

| | |
|-|-|
| **Plugin Title** | Security Solution Logging |
| **Cloud** | AZURE |
| **Category** | Log Alerts |
| **Description** | Ensures Activity Log Alerts for the create or update and delete Security Solution events are enabled |
| **More Info** | Monitoring for create or update and delete Security Solution events gives insight into event changes and may reduce the time it takes to detect suspicious activity. |
| **AZURE Link** | https://docs.microsoft.com/en-us/azure/security/azure-log-audit |
| **Recommended Action** | Add a new log alert to the Alerts service that monitors for Security Solution create or update and delete events. |

## Detailed Remediation Steps

1. Log into the Microsoft Azure Management Console.
2. In the search bar at the top, search for **Monitor** and select it. </br> <img src="/resources/azure/logalerts/security-solution-logging/step2.png"/>
3. In the left pane, select **Alerts**, then click **+ Create** and select **Alert rule**. </br> <img src="/resources/azure/logalerts/security-solution-logging/step3.png"/>
4. On the **Select a resource** pane, filter by your **Subscription** and select the target subscription. Leave the **Resource type** filter blank or set to **All** - do not attempt to filter by Security Solutions here, as it is a legacy provider that may not appear. Click **Apply**. </br> <img src="/resources/azure/logalerts/security-solution-logging/step4.png"/>
5. On the **Condition** tab, click **See all signals**. In the **Select a signal** pane, search for and select the following operations:
   - **Create or Update Security Solutions (Microsoft.Security/securitySolutions/write)**
   - **Delete Security Solutions (Microsoft.Security/securitySolutions/delete)**
   - If the above signals do not appear, select **All Administrative operations** and manually set the **Operation name** to `Microsoft.Security/securitySolutions/write` (for create/update) or `Microsoft.Security/securitySolutions/delete` (for delete). </br> <img src="/resources/azure/logalerts/security-solution-logging/step5.png"/>
6. In the **Alert logic** section, set the following:
   - **Event level**: Select **All** (or specific levels such as Informational or Warning).
   - **Status**: Select **All** (to monitor all changes).
   - **Event initiated by**: Leave as **All services and users**. </br> <img src="/resources/azure/logalerts/security-solution-logging/step6.png"/>
7. On the **Actions** tab, click **Use action groups** to choose an existing group. </br> <img src="/resources/azure/logalerts/security-solution-logging/step7.png"/>
8. On the **Details** tab, select a **Resource group** where the alert rule itself will be stored. Provide an **Alert rule name** (e.g., `Alert - Security Solution Changes`) and an **Alert rule description**. </br> <img src="/resources/azure/logalerts/security-solution-logging/step8.png"/>
9. Click **Review + create**, select **Enable upon creation** to activate the alert immediately, then click **Create**. </br> <img src="/resources/azure/logalerts/security-solution-logging/step9.png"/>
