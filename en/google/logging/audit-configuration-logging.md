[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)

# GOOGLE / Logging / Audit Configuration Logging

## Quick Info

| | |
|-|-|
| **Plugin Title** | Audit Configuration Logging |
| **Cloud** | GOOGLE |
| **Category** | Logging |
| **Description** | Ensures that logging and log alerts exist for audit configuration changes. |
| **More Info** | Project Ownership is the highest level of privilege on a project, any changes in audit configuration should be heavily monitored to prevent unauthorized changes. |
| **GOOGLE Link** | https://cloud.google.com/logging/docs/logs-based-metrics/ |
| **Recommended Action** | Ensure that log alerts exist for audit configuration changes. |

## Detailed Remediation Steps
1. Log in to the [Google Cloud Platform Console](console.cloud.google.com).
2. From the top navigation bar, select the GCP project you want to work with.
3. Navigate to the [Log-based Metrics](https://console.cloud.google.com/logs/metrics) page in the Google Cloud Console.<br> <img src="/resources/google/logging/audit-configuration-logging/step3.png"/>
4. On the Logs-based Metrics page, within the User-defined metrics area, click in the `Filter` box, select `Filter`, and enter the following filter pattern, the press Enter to check if this filter already exists.
```
resource.type=global AND protoPayload.methodName=SetIamPolicy AND protoPayload.serviceData.policyDelta.auditConfigDeltas:*
```
<br> <img src="/resources/google/logging/audit-configuration-logging/step4.png"/>
5. Click `Create metric` next to User-defined metrics to add a new log metric using the filter pattern you just entered.<br> <img src="/resources/google/logging/audit-configuration-logging/step5.png"/>

6. On the log-based metric creation page, follow these steps: <br>
   a. Set the `Metric Type` to `Counter`. <br>
   b. In the Details section, enter a unique name for your log metric, add a brief description of its purpose, and set the `Units` field to `1` to count each matching log entry.<br>
   c. For the `Filter Selection`, make sure the log scope is set to `Project logs`. Then, paste the following filter into the `Build filter` box:
```
resource.type=global AND protoPayload.methodName=SetIamPolicy AND protoPayload.serviceData.policyDelta.auditConfigDeltas:*
```
   d. (Optional) To add labels, click + Add label and include any tags you want, then click Done.<br> <img src="/resources/google/logging/audit-configuration-logging/step6-1.png"/> <br>
   e. Click Create metric to finish. If successful, you’ll see a confirmation that your log metric was created and data will be available soon.<br>
<br> <img src="/resources/google/logging/audit-configuration-logging/step6-2.png"/>

7. In the left-side menu, go to Logs-based Metrics under the Configure section.<br> <img src="/resources/google/logging/audit-configuration-logging/step7.png"/>
8. Find your new log metric in the `User-defined metrics` list and confirm it is enabled (look for a green checkmark). Click the three dots next to the metric to open more options, then select `Create alert from metric` to set up an alerting policy based on this metric.<br> <img src="/resources/google/logging/audit-configuration-logging/step8.png"/>
9. You’ll need to define a condition for the alerting policy before it can trigger notifications. On the alerting policy setup page, follow these steps:
  * Under New condition, enter the required details: 
    1. Set the Policy configuration mode to Builder.
    2. Confirm that the correct metric appears in the Select a metric field; this should automatically display the metric you created earlier.
    3. In the Transform data section, set the `Rolling window` to your preferred duration (such as 10 minutes), choose `delta` for the window function, and set `Time series aggregation` to `count`.
    4. (Optional) To group time series by label, click in the `Time series group by` box and select a label from the list.
    5. Click `NEXT` to proceed with the setup.<br>

<br> <img src="/resources/google/logging/audit-configuration-logging/step9.png"/>

  * For `Configure trigger`, follow these steps:
    1. Set the Condition Type to Threshold.
    2. For Alert trigger, select Any time series violates.
    3. Set Threshold position to Above threshold.
    4. Enter 0 as the threshold value. This will trigger an alert for every audit configuration change detected in the selected GCP project.
    5. Enter a unique name for this condition in the Condition name field.
    6. Click NEXT to continue with the setup.
<br> <img src="/resources/google/logging/audit-configuration-logging/step9-2.png"/>

  * For `Notifications and name`, follow these steps:
    1. Enable notification channels so you can receive alerts. Select the channels where you want to get notified, such as email addresses.
    2. In the `Notification Channels` field, pick the channels you want to use for alerts and click OK to save. To add a new channel, select `Manage Notification Channels` and create one. It’s a good idea to set up more than one channel for reliability.
    3. (Optional) For `Notify on incident closure`, decide if you want notifications when an incident closes and set the time after which an incident will close automatically if no data is received.
    4. (Optional) Use `+ Add Label` to attach custom labels to your alert policy for better organization.
    5. (Optional) Set the `Policy Severity Level` to help prioritize alerts.
    6. (Optional) Add any relevant information in the `Documentation` field; this will be included in alert notifications.
    7. Enter a clear and descriptive name for your alerting policy in the `Name the alert policy` field.
    8. Click `NEXT` to continue.
<br> <img src="/resources/google/logging/audit-configuration-logging/step9-3.png"/> 