# AZURE / Log Alerts / Virtual Network Alerts Monitor

## Quick Info

| | |
|-|-|
| **Plugin Title** | Virtual Network Alerts Monitor |
| **Cloud** | AZURE |
| **Category** | Log Alerts |
| **Description** | Ensures Activity Log Alerts for the create or update and delete Virtual Networks events are enabled |
| **More Info** | Monitoring for create or update and delete Virtual Networks events gives insight into event changes and may reduce the time it takes to detect suspicious activity. |
| **AZURE Link** | https://learn.microsoft.com/en-us/azure/virtual-network/monitor-virtual-network#alerts |
| **Recommended Action** | Add a new log alert to the Alerts service that monitors for Virtual Networks create or update and delete events. |

## Detailed Remediation Steps

1. Log into the Microsoft Azure Management Console.
2. Click on the "Search resources, services, and docs" search bar at the top and search for "Alerts". </br> <img src="/resources/azure/logalerts/virtual-network-alerts-monitor/step2.png"/>
3. On the "Alerts" page, click on the "Alert rules" button at the top panel.</br> <img src="/resources/azure/logalerts/virtual-network-alerts-monitor/step3.png"/>
4. On the "Alert Rules" page, find the "Target Resource Type" column in the table and check if there are any rules with "Virtual network" in that column. If there are no "Alerts" configured then "Activity Log Alerts" for the create or update and delete "Virtual Networks" events are not enabled.</br> <img src="/resources/azure/logalerts/virtual-network-alerts-monitor/step4.png"/>
5. Repeat steps 2 - 4 to check other Azure accounts.</br>
6. Navigate to the "Alerts" page and click on the "Create" dropdown, at the top, and select the "Alert rule" option.</br> <img src="/resources/azure/logalerts/virtual-network-alerts-monitor/step6.png"/>
7. On the "Create an alert rule" page, click on the "Select scope" option on the left side of the page and a side page will appear on the right side. Under "select a resource" navigate to the "Resource types" dropdown and search for "Virtual networks". Select the "Resource" accordingly. Once you are done selecting your resources, click the "Apply" button at the bottom of the side page. After this, click on the "Next: Condition >" button at the bottom left of the page.</br> <img src="/resources/azure/logalerts/virtual-network-alerts-monitor/step7.png"/>
8. On the "Create an alert rule" page, click on the "See all signals" option, located under the "Signal name" dropdown, and select "All Administrative operations" from the options and click on the "Apply" option at the bottom of the tab. After this, click on the "Next: Actions >" button.</br> <img src="/resources/azure/logalerts/virtual-network-alerts-monitor/step8.png"/>
9. Under the "Actions" tab, either click on "Select action groups" or "Create action group" accordingly. After this is done, click on the "Next: Details >" button.</br> <img src="/resources/azure/logalerts/virtual-network-alerts-monitor/step9.png"/>
10. Enter the "Alert rule name" and "Alert rule description" under the "Details" tab. Expand the "Advanced option" dropdown and click on the checkbox for the "Enable alert rule upon creation" option to quickly enable the "Virtual Network Alerts Monitor". Click on the "Review + create" button at the bottom left of the page. Then click on the "Create" button at the bottom left of the page to create a rule.</br> <img src="/resources/azure/logalerts/virtual-network-alerts-monitor/step10.png"/>
11. Repeat steps 6 - 10 to add a new log alert to the Alerts service that monitors for Virtual Networks create or update and delete events.</br>
