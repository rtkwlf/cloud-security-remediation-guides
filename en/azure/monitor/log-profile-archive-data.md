# AZURE / Monitor / Log Profile Archive Data

### Quick Info

| | |
|-|-|
| **Plugin Title** | Log Profile Archive Data |
| **Cloud** | AZURE |
| **Category** | Monitor |
| **Description** | Ensures the Log Profile is configured to export all activities from the control and management planes in all active locations |
| **More Info** | Exporting log activity for control plane activity allows for audited access to the Azure account with event data in the case of a security incident. |
| **AZURE Link** | https://docs.microsoft.com/en-us/azure/azure-monitor/platform/archive-activity-log |
| **Recommended Action** | Ensure that all activity is logged to the Event Hub or storage account for archiving. |


### Prerequisites

Before proceeding with the remediation steps, ensure you have the following:

*   **Storage Account (if archiving to storage):** An existing Azure Storage account (General-purpose v2 recommended) with `ListKeys` permissions for the user configuring the diagnostic setting. This storage account should *not* be the same one you are monitoring if you are also collecting resource logs for that storage account.
*   **Event Hub Namespace and Event Hub (if streaming to Event Hub):** An existing Azure Event Hubs namespace and an Event Hub within it, if you plan to stream logs to an Event Hub.

### Detailed Remediation Steps

Follow these steps to configure or update diagnostic settings for the Azure Activity Log to ensure all activities are logged to an Event Hub or storage account for archiving. This process uses **Diagnostic Settings**, which is the current and recommended method for exporting Activity Logs, replacing the older "Log Profiles" mentioned in the detection script.

1.  **Sign in to the Azure portal:** Go to [https://portal.azure.com](https://portal.azure.com).

2.  **Navigate to Activity Log:**
    *   In the Azure portal search bar, type "Monitor" and select **Monitor** from the services. </br> <img src="/resources/azure/monitor/log-profile-archive-data/step2_1.png"/>
    *   From the Azure Monitor menu, select **Activity log**.</br> <img src="/resources/azure/monitor/log-profile-archive-data/step2_2.png"/>

3.  **Access Diagnostic Settings:**
    *   On the Activity log page, select **Export Activity Logs** (this will take you to the Diagnostic settings page for the Activity Log). </br> <img src="/resources/azure/monitor/log-profile-archive-data/step3.png"/>

4.  **Add or Edit a Diagnostic Setting:**
    *   If no diagnostic setting exists, click **+ Add diagnostic setting**.
    *   If an existing diagnostic setting needs modification, select **Edit setting** next to the relevant setting. </br> <img src="/resources/azure/monitor/log-profile-archive-data/step4.png"/>

5.  **Configure Diagnostic Setting Details:**
    *   **Diagnostic setting name:** Enter a descriptive name for your diagnostic setting (e.g., `ActivityLogArchiveToStorage` or `ActivityLogStreamToEventHub`).
    *   **Log categories:** Under "Log categories", select all relevant categories to ensure comprehensive logging. Based on the detection script, ensure at least the following are selected:
        *   **Administrative** (covers 'Write', 'Delete', 'Action' operations)
        *   **Security**
        *   **ServiceHealth**
        *   **Alert**
        *   **Autoscale**
        *   **Recommendation**
        *   **Policy**
        *   **ResourceHealth**

6.  **Choose Destination Details:**
    * **Storage Account** is ideal for **long-term, cost-effective storage**, auditing, or retention.
    * **Event Hub** is designed for **real-time ingestion and streaming** to downstream systems like SIEMs, analytics platforms, or other applications.
    *   **To archive to a Storage Account:**
        *   Select the **Archive to a storage account** checkbox.
        *   Choose the appropriate **Subscription** where your storage account resides.
        *   Select the **Storage account** from the dropdown list that you prepared in the prerequisites.
    *   **To stream to an Event Hub:**
        *   Select the **Stream to an Event Hub** checkbox.
        *   Choose the appropriate **Subscription** where your Event Hub namespace resides.
        *   Select the **Event hub namespace** from the dropdown list.
        *   Select the **Event hub name** from the dropdown list.
        *   Select the **Event hub policy name** from the dropdown list.

7.  **Save the Diagnostic Setting:**
    *   Click **Save** to apply your diagnostic setting. </br> <img src="/resources/azure/monitor/log-profile-archive-data/step5_and_6_and_7.png"/>

### Verification

After configuring the diagnostic setting, verify that logs are being correctly sent to your chosen destination.

1.  **Verify Diagnostic Setting Status:**
    *   Navigate back to **Azure Monitor** > **Activity log** > **Export Activity Logs**.
    *   Confirm that your newly created or updated diagnostic setting is listed and shows an "Enabled" status.

### Next Steps

*   **Implement Storage Lifecycle Management:** If you are archiving to a storage account, configure a [lifecycle management policy](https://learn.microsoft.com/en-us/azure/storage/blobs/lifecycle-management-overview) on the storage account to automatically manage the retention and tiering of your archived Activity Logs, optimizing costs and ensuring compliance with retention requirements.
