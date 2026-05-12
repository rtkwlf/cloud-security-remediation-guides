# AZURE / Log Alerts / Security Policy Alerts Enabled

## Quick Info

| | |
|-|-|
| **Plugin Title** | Security Policy Alerts Enabled |
| **Cloud** | AZURE |
| **Category** | Log Alerts |
| **Description** | Ensures Activity Log alerts for create or update Security Policy Rule events are enabled |
| **More Info** | Monitoring for create or update Security Policy Rule events gives insight into policy changes and may reduce the time it takes to detect suspicious activity. |
| **AZURE Link** | https://learn.microsoft.com/en-us/azure/azure-monitor/alerts/alerts-types |
| **Recommended Action** | Add a new log alert to the Alerts service that monitors for Security Policy Rule create or update events. |

## Detailed Remediation Steps

#### Start the Alert Creation

1. Log in to the [Azure Portal](https://portal.azure.com/).
2. Search for **Monitor** in the top search bar and select it. </br> <img src="/resources/azure/logalerts/security-policy-alerts-enabled/step1.png"/>
3. In the left-hand menu, select **Alerts**, then click **+ Create** and select **Alert rule**. </br> <img src="/resources/azure/logalerts/security-policy-alerts-enabled/step2.png"/>

#### Set the Scope

1. On the **Scope** tab, under **Select a resource** pane. </br> <img src="/resources/azure/logalerts/security-policy-alerts-enabled/step3.png"/>, Filter by your **Subscription**.
3. For **Resource type**, select **Network security groups** (or select the entire subscription to monitor all NSGs within it).
4. Click **Apply**.

#### Configure the Condition

1. Select the **Condition** tab and click **See all signals**. </br> <img src="/resources/azure/logalerts/security-policy-alerts-enabled/step4.png"/>
2. Search for and select the signal: **Create or Update Security Rule (Microsoft.Network/networkSecurityGroups/securityRules/write)** and click **Apply**.
3. In the **Alert logic** pane, set the following: </br> <img src="/resources/azure/logalerts/security-policy-alerts-enabled/step5.png"/>
   - **Event level**: Select **All** (or specific levels such as Informational or Warning).
   - **Status**: Select **All Selected** (to monitor all changes).
   - **Event initiated by**: Leave as **All services and users**.

#### Link an Action Group

1. Go to the **Actions** tab. </br> <img src="/resources/azure/logalerts/security-policy-alerts-enabled/step6.png"/>
2. Click **Use action groups** to choose an existing group, or **Use quick actions** to set up a new one (e.g., to send an email or SMS notification) and click **Select**.

#### Finalise Details

1. On the **Details** tab: </br> <img src="/resources/azure/logalerts/security-policy-alerts-enabled/step7.png"/>
2. Select a **Resource group** where the alert rule itself will be stored.
3. Provide an **Alert rule name** (e.g., `NSG-Rule-Change-Alert`) and a **Alert rule description**.
4. Under **Advanced details**, check **Enable alert rule upon creation** to activate the alert immediately.
5. Click **Review + create**, then click **Create**.
