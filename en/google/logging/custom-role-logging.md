[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)

# GOOGLE / Logging / Custom Role Logging

## Quick Info

| | |
|-|-|
| **Plugin Title** | Custom Role Logging |
| **Cloud** | GOOGLE |
| **Category** | Logging |
| **Description** | Ensures that logging and log alerts exist for custom role creation and changes |
| **More Info** | Project Ownership is the highest level of privilege on a project, any changes in custom role should be heavily monitored to prevent unauthorized changes. |
| **GOOGLE Link** | https://cloud.google.com/logging/docs/logs-based-metrics/ |
| **Recommended Action** | Ensure that log alerts exist for custom role creation and changes. |

## Detailed Remediation Steps
To ensure that log alerts exist for custom role creation and changes in GCP, you should create a log-based metric and an alerting policy that monitors for these events. <br>
Create a Log-Based Metric:
  1. Navigate to the [Log-based Metrics](https://console.cloud.google.com/logs/metrics) page in the Google Cloud Console.<br> <img src="/resources/google/logging/custom-role-logging/step1-a.png"/>
  2. On the Logs-based Metrics page, in the User-defined metrics section, click on the Filter Box, select Filter, paste the following filter pattern, and press Enter. This is to ensure that there is no such filter pattern already available.
  ```
  resource.type=iam_role AND protoPayload.methodName=google.iam.admin.v1.CreateRole OR protoPayload.methodName=google.iam.admin.v1.DeleteRole OR protoPayload.methodName=google.iam.admin.v1.UpdateRole
  ``` 
  <br> <img src="/resources/google/logging/custom-role-logging/step1-b-1.png"/><br> <img src="/resources/google/logging/custom-role-logging/step1-b-2.png"/>

  3. Click `Create metric` next to User-defined metrics to create a new log metric based on the filter pattern specified at the previous step. <br> <img src="/resources/google/logging/custom-role-logging/step1-c.png"/>
  4. On the "Create log-based metric" page, perform the following steps:
    - Set the `Metric Type` to `Counter`.
    - In the Details section, enter a distinctive name for your log metric, add a brief description explaining its purpose, and set the `Units` field to `1` to count matching log entries.
    - For the `Filter selection`, make sure the log scope is set to `Project logs`. Then, paste the following filter into the filter box:
    ```
    resource.type=iam_role AND protoPayload.methodName=google.iam.admin.v1.CreateRole OR protoPayload.methodName=google.iam.admin.v1.DeleteRole OR protoPayload.methodName=google.iam.admin.v1.UpdateRole
    ```

    - (Optional) To add labels, click `+ Add label` and include any tags you want, then click Done.
    - Click `Create metric` to finish. If successful, you’ll see a confirmation that your log metric was created and data will be available shortly.<br> <img src="/resources/google/logging/custom-role-logging/step1-d.png"/>

    - In the left-side menu, go to Logs-based Metrics under the Configure section.<br> <img src="/resources/google/logging/custom-role-logging/step1-e.png"/>
    - Find your new log metric in the User-defined metrics list and confirm it is enabled (look for a green checkmark). Click the three dots next to the metric to open more options, then select Create alert from metric to set up an alerting policy based on this metric.<br> <img src="/resources/google/logging/custom-role-logging/step1-f.png"/>

    - Before you can set up an alert, you need to define a condition that will trigger the alerting policy. On the alerting policy creation page, follow these steps: 
      * Under New condition, fill in the required details:
        1. Set the Policy configuration mode to `Builder`.
        2. Make sure the correct metric appears in the Select a metric field. This should automatically show the metric you created earlier.
        3. In the Transform data section, set the `Rolling window` to your preferred duration (for example, 10 minutes), choose delta for the `Rolling window function`, and set Time series aggregation to count.
        4. (Optional) If you want to group time series by label, click in the `Time series group by` box and select the desired label from the list.
        5. Click `NEXT` to proceed with the alert setup.
            
    <br> <img src="/resources/google/logging/custom-role-logging/step*.png"/>  

      * For `Configure trigger`, follow these steps:
        1. Set the Condition Type to Threshold.
        2. For Alert trigger, choose Any time series violates.
        3. Set Threshold position to Above threshold.
        4. Enter 0 as the threshold value. This ensures an alert is generated for every custom IAM role change in the selected GCP project.
        5. Enter a unique name for the condition in the Condition name field.
        6. Click NEXT to proceed.
    <br> <img src="/resources/google/logging/custom-role-logging/step*2.png"/>  
        
      * For `Notifications and name`, follow these steps:
        1. Enable the option to use notification channels so you can receive alerts. Select the channels where you want to be notified, such as email addresses.
        2. In the Notification Channels field, pick the channels you want to use for alerts. Click OK to confirm. If you need to add a new channel, select Manage Notification Channels and create one. For better reliability, it’s recommended to set up more than one notification channel.
        3. (Optional) For Notify on incident closure, decide if you want to be notified when an incident is closed and set the duration for automatic closure if no data is received.
        4. (Optional) Use + Add Label to attach custom labels to your alert policy for easier organization. 
        5. (Optional) Set the Policy Severity Level to help prioritize alerts.
        6. (Optional) Add any relevant information in the Documentation field; this will be included in alert notifications.
        7. Enter a clear and descriptive name for your alerting policy in the Name the alert policy field.
        8. Click NEXT to continue.

      * For `Review Alert`, verify the alert policy settings, then click `CREATE POLICY` to finalize and activate the new alerting policy. This will allow you to monitor custom IAM role changes in the chosen GCP project.
