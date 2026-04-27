# AZURE / Log Alerts / Policy Assignment Alerts Enabled

## Quick Info

| | |
|-|-|
| **Plugin Title** | Policy Assignment Alerts Enabled |
| **Cloud** | AZURE |
| **Category** | Log Alerts |
| **Description** | Ensures Activity Log alerts for create or update and delete Policy Assignment events are enabled |
| **More Info** | Monitoring for create or update and delete Policy Assignment events gives insight into policy changes and may reduce the time it takes to detect suspicious activity. |
| **AZURE Link** | https://learn.microsoft.com/en-us/azure/azure-monitor/platform/activity-log-alerts |
| **Recommended Action** | Add a new log alert to the Alerts service that monitors for Policy Assignment create or update and delete events. |

---

## Detailed Remediation Steps

#### Create Activity Log Alert for Policy Assignment Operations

1. In the [Azure portal](https://portal.azure.com/), select **Monitor**.
2. On the left pane, select **Alerts**.
3. Select **+ Create** > **Alert rule**.
4. On the **Select a resource** pane, set the scope to your subscription and select **Apply**.
5. On the **Condition** tab, Click **Select a signal** dropdown, search for and select signals related to Policy Assignment operations, such as:
   - **Policy Assignment Create or Update**: Search for **Create Policy Assignment** to capture events when a Policy Assignment is created or updated.
   - **Policy Assignment Delete**: Search for **Delete Policy Assignment** to capture events when a Policy Assignment is deleted.
6. Select **Apply**.

#### Add Filter Conditions for Policy Assignment Operations

1. On the **Conditions** pane, select the **Chart period** value to preview the results.
2. In the **Alert logic** section, configure the following fields:
   - **Event level**: Select **All** to capture all severity levels
   - **Status**: Select **All** to monitor all Policy Assignment events regardless of their status
   - **Event initiated by**: Leave blank or select specific principals as needed

#### Attach Action Group to Alert Rule

1. Select the **Actions** tab.
2. Select or create an action group to define notifications and automated actions that will be triggered when the alert fires.
3. Click **Select** to confirm the action group selection.

#### Enable Alert Rule Upon Creation

1. Select the **Details** tab.
2. Enter a descriptive **Alert rule name** (for example, "Policy Assignment Activity Log Alert").
3. Enter an **Alert rule description** if desired.
4. Check the **Enable alert rule upon creation** checkbox under **Advanced options** to ensure the alert rule starts running immediately after creation.
5. Select the **Review + Create** tab to review your settings, and then select the **Create** button to create the alert rule.

## Verification Steps

#### Verify Activity Log Alert Exists for Policy Assignment Operations

1. In the [Azure portal](https://portal.azure.com/), select **Monitor**.
2. On the left pane, select **Alerts**.
3. Select **Alert rules**.
4. Confirm the newly created activity log alert for Policy Assignment operations is present and visible in the list.

#### Verify Alert Rule Conditions and Filters

1. Select the alert rule you created from the **Alert rules** list.
2. Select **Edit** to open the alert rule details.
3. Select the **Condition** tab and check the **Condition preview** to verify the Signal name is correct:
   - For the Create/Update rule: confirm the Signal name shows **Create Policy Assignment**.
   - For the Delete rule: confirm the Signal name shows **Delete Policy Assignment**.
4. Confirm the **Event level**, **Status**, and **Event initiated by** settings match your configuration.
5. Select the **X** [Cancel] button at the top right corner or navigate away without saving.

#### Verify Alert Rule is Enabled

1. From the **Alert rules** list, find the alert rule you created.
2. Confirm that the rule status shows as **Enabled**.