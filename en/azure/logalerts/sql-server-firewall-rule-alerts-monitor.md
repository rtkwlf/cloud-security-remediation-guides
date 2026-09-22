# AZURE / Log Alerts / SQL Server Firewall Rule Alerts Monitor

## Quick Info

| | |
|-|-|
| **Plugin Title** | SQL Server Firewall Rule Alerts Monitor |
| **Cloud** | AZURE |
| **Category** | Log Alerts |
| **Description** | Ensures Activity Log Alerts for the create or update and delete SQL Server Firewall Rules events are enabled |
| **More Info** | Monitoring for create or update and delete SQL Server Firewall Rules events gives insight into event changes and may reduce the time it takes to detect suspicious activity. |
| **AZURE Link** | https://learn.microsoft.com/en-us/azure/azure-sql/database/firewall-configure |
| **Recommended Action** | Add a new log alert to the Alerts service that monitors for SQL Server Firewall Rules create or update and delete events. |

## Detailed Remediation Steps

1. Log into the Microsoft Azure Management Console.
2. In the search bar at the top, search for **Monitor** and select it. </br> <img src="/resources/azure/logalerts/sql-server-firewall-rule-alerts-monitor/step2.png"/>
3. In the left pane, select **Alerts**, then click **+ Create** and select **Alert rule**. </br> <img src="/resources/azure/logalerts/sql-server-firewall-rule-alerts-monitor/step3.png"/>
4. On the **Select a resource** pane, filter by your **Subscription** and select the subscription or resource group containing your SQL servers. Click **Apply**. </br> <img src="/resources/azure/logalerts/sql-server-firewall-rule-alerts-monitor/step4.png"/>
5. On the **Condition** tab, click **See all signals**. In the **Select a signal** pane, search for and select the following operations: </br> <img src="/resources/azure/logalerts/sql-server-firewall-rule-alerts-monitor/step5.png"/>
   - **Create/Update server firewall rule (Server Firewall Rule)**
   - **Delete server firewall rule (Server Firewall Rule)**
6. In the **Alert logic** section, set the following: </br> <img src="/resources/azure/logalerts/sql-server-firewall-rule-alerts-monitor/step6.png"/>
   - **Event level**: Select **All** (or specific levels such as Informational or Warning).
   - **Status**: Select **All** (to monitor all changes).
   - **Event initiated by**: Leave as **All services and users**.
7. On the **Actions** tab, click **Use action groups** to choose an existing group. </br> <img src="/resources/azure/logalerts/sql-server-firewall-rule-alerts-monitor/step7.png"/>
8. On the **Details** tab, select a **Resource group** where the alert rule itself will be stored. Provide an **Alert rule name** (e.g., `Alert - SQL Server Firewall Rule Changes`) and an **Alert rule description**. </br> <img src="/resources/azure/logalerts/sql-server-firewall-rule-alerts-monitor/step8.png"/>
9. Click **Review + create**, select **Enable upon creation** to activate the alert immediately, then click **Create**. </br> <img src="/resources/azure/logalerts/sql-server-firewall-rule-alerts-monitor/step9.png"/>
