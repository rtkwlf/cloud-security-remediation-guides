# AWS / CloudWatchLogs / CloudWatch Monitoring Metrics

## Quick Info

| | |
|-|-|
| **Plugin Title** | CloudWatch Monitoring Metrics |
| **Cloud** | AWS |
| **Category** | CloudWatchLogs |
| **Description** | Ensures metric filters are setup for CloudWatch logs to detect security risks from CloudTrail. |
| **More Info** | Sending CloudTrail logs to CloudWatch is only useful if metrics are setup to detect risky activity from those logs. There are numerous metrics that should be used. For the exact filter patterns, please see this plugin on GitHub: https://github.com/cloudsploit/scans/blob/master/plugins/aws/cloudwatchlogs/monitoringMetrics.js |
| **AWS Link** | https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/MonitoringLogData.html |
| **Recommended Action** | Enable metric filters to detect malicious activity in CloudTrail logs sent to CloudWatch. |

## Prerequisites

Before proceeding, ensure the following:

*   **AWS CloudTrail Enabled**: You have an active CloudTrail trail configured to log management and/or data events.
*   **CloudTrail to CloudWatch Logs Integration**: Your CloudTrail trail is configured to send logs to a CloudWatch Logs log group. If not, please refer to the AWS documentation on [Sending CloudTrail Events to CloudWatch Logs](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/send-cloudtrail-events-to-cloudwatch-logs.html) to set this up.
*   **IAM Permissions**: You have the necessary IAM permissions to:
    *   Access the CloudWatch console and your CloudWatch Logs log groups.
    *   Create and manage CloudWatch Metric Filters.
    *   Create and manage CloudWatch Alarms (recommended for next steps).

## Detailed Remediation Steps

This section outlines the steps to create each recommended CloudWatch Metric Filter using the AWS Console. You will create a separate metric filter for each pattern.

**General Steps for Creating a Metric Filter (repeated for each pattern below):**

1.  Open the CloudWatch console at [https://console.aws.amazon.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/).
2.  In the navigation pane, choose **Logs**, and then choose **Log groups**.  </br> <img src="/resources/aws/cloudwatchlogs/cloudwatch-monitoring-metrics/step2.png"/>
3.  Select the name of the CloudWatch Logs log group that your CloudTrail trail is sending logs to.
4.  Choose the `Actions` dropdown, and then choose **Create metric filter**. </br> <img src="/resources/aws/cloudwatchlogs/cloudwatch-monitoring-metrics/step3_and_4.png"/>
5.  For **Filter pattern**, enter the specific filter pattern provided for each security control below.
6.  (Optional but Recommended) To test your filter pattern, under **Test Pattern**, enter one or more sample log events that would match the pattern.
7.  Choose **Next**.  </br> <img src="/resources/aws/cloudwatchlogs/cloudwatch-monitoring-metrics/step5_and_7.png"/>
8.  For **Metric filter name**, enter a descriptive name for the filter.
9.  Under **Metric details**:
    *   For **Metric namespace**, enter a consistent namespace, e.g., `CloudTrailMetrics`.
    *   For **Metric name**, enter a unique and descriptive name for the metric (e.g., `UnauthorizedAPICallsCount`).
    *   For **Metric value**, enter `1`. This increments the metric by 1 for each log event that matches the filter pattern.
    *   (Optional) For **Unit**, select `Count`. </br> <img src="/resources/aws/cloudwatchlogs/cloudwatch-monitoring-metrics/step8_and_9.png"/>
10.  Choose **Next**. </br> <img src="/resources/aws/cloudwatchlogs/cloudwatch-monitoring-metrics/step10.png"/>
11. Choose **Create metric filter**. </br> <img src="/resources/aws/cloudwatchlogs/cloudwatch-monitoring-metrics/step11.png"/>

Repeat these steps for each of the following security-focused metric filters:

---

### (1) Unauthorized API Calls

Detects attempts to perform actions that are not authorized.

*   **Filter Name**: `Unauthorized API Calls`
*   **Filter Pattern**: `{($.errorCode = *UnauthorizedOperation) || ($.errorCode = AccessDenied*)}`
*   **Metric Namespace**: `CloudTrailMetrics`
*   **Metric Name**: `UnauthorizedAPICallsCount`

---

### (2) Sign In Without MFA

Identifies console logins where Multi-Factor Authentication (MFA) was not used.

*   **Filter Name**: `Sign In Without MFA`
*   **Filter Pattern**: `{($.eventName = ConsoleLogin) && ($.additionalEventData.MFAUsed != Yes)}`
*   **Metric Namespace**: `CloudTrailMetrics`
*   **Metric Name**: `SignInWithoutMFACount`

---

### (3) Root Account Usage

Monitors for activities performed by the AWS Root user, which should be minimized.

*   **Filter Name**: `Root Account Usage`
*   **Filter Pattern**: `{($.userIdentity.type = Root && $.userIdentity.invokedBy NOT EXISTS && $.eventType != AwsServiceEvent)}`
*   **Metric Namespace**: `CloudTrailMetrics`
*   **Metric Name**: `RootAccountUsageCount`

---

### (4) IAM Policy Changes

Detects changes to IAM policies, which can indicate privilege escalation or unauthorized modifications.

*   **Filter Name**: `IAM Policy Changes`
*   **Filter Pattern**: `{($.eventName=DeleteGroupPolicy)||($.eventName=DeleteRolePolicy)||($.eventName=DeleteUserPolicy)||($.eventName=PutGroupPolicy)||($.eventName=PutRolePolicy)||($.eventName=PutUserPolicy)||($.eventName=CreatePolicy)||($.eventName=DeletePolicy)||($.eventName=CreatePolicyVersion)||($.eventName=DeletePolicyVersion)||($.eventName=AttachRolePolicy)||($.eventName=DetachRolePolicy)||($.eventName=AttachUserPolicy)||($.eventName=DetachUserPolicy)||($.eventName=AttachGroupPolicy)||($.eventName=DetachGroupPolicy)}`
*   **Metric Namespace**: `CloudTrailMetrics`
*   **Metric Name**: `IAMPolicyChangesCount`

---

### (5) CloudTrail Configuration Changes

Monitors for any modifications to CloudTrail configurations, which could indicate an attempt to hide malicious activity.

*   **Filter Name**: `CloudTrail Configuration Changes`
*   **Filter Pattern**: `{($.eventName = CreateTrail) || ($.eventName = UpdateTrail) || ($.eventName = DeleteTrail) || ($.eventName = StartLogging) || ($.eventName = StopLogging)}`
*   **Metric Namespace**: `CloudTrailMetrics`
*   **Metric Name**: `CloudTrailConfigChangesCount`

---

### (6) Sign In Failures

Detects failed console login attempts, which could indicate brute-force attacks.

*   **Filter Name**: `Sign In Failures`
*   **Filter Pattern**: `{($.eventName = ConsoleLogin) && ($.errorMessage = "Failed authentication")}`
*   **Metric Namespace**: `CloudTrailMetrics`
*   **Metric Name**: `SignInFailuresCount`

---

### (7) Disabled CMKs

Monitors for attempts to disable or schedule deletion of AWS Key Management Service (KMS) Customer Master Keys (CMKs).

*   **Filter Name**: `Disabled CMKs`
*   **Filter Pattern**: `{($.eventSource = kms.amazonaws.com) && (($.eventName=DisableKey)||($.eventName=ScheduleKeyDeletion))}`
*   **Metric Namespace**: `CloudTrailMetrics`
*   **Metric Name**: `DisabledCMKsCount`

---

### (8) S3 Policy Changes

Detects changes to S3 bucket policies, ACLs, or other configurations that could expose data.

*   **Filter Name**: `S3 Policy Changes`
*   **Filter Pattern**: `{($.eventSource = s3.amazonaws.com) && (($.eventName = PutBucketAcl) || ($.eventName = PutBucketPolicy) || ($.eventName = PutBucketCors) || ($.eventName = PutBucketLifecycle) || ($.eventName = PutBucketReplication) || ($.eventName = DeleteBucketPolicy) || ($.eventName = DeleteBucketCors) || ($.eventName = DeleteBucketLifecycle) || ($.eventName = DeleteBucketReplication))}`
*   **Metric Namespace**: `CloudTrailMetrics`
*   **Metric Name**: `S3PolicyChangesCount`

---

### (9) ConfigService Changes

Monitors for changes to AWS Config service configurations, which could impact compliance monitoring.

*   **Filter Name**: `ConfigService Changes`
*   **Filter Pattern**: `{($.eventSource = config.amazonaws.com) && (($.eventName=StopConfigurationRecorder)||($.eventName=DeleteDeliveryChannel)||($.eventName=PutDeliveryChannel)||($.eventName=PutConfigurationRecorder))}`
*   **Metric Namespace**: `CloudTrailMetrics`
*   **Metric Name**: `ConfigServiceChangesCount`

---

### (10) Security Group Changes

Detects modifications to EC2 Security Groups, which can impact network access.

*   **Filter Name**: `Security Group Changes`
*   **Filter Pattern**: `{($.eventName = AuthorizeSecurityGroupIngress) || ($.eventName = AuthorizeSecurityGroupEgress) || ($.eventName = RevokeSecurityGroupIngress) || ($.eventName = RevokeSecurityGroupEgress) || ($.eventName = CreateSecurityGroup) || ($.eventName = DeleteSecurityGroup)}`
*   **Metric Namespace**: `CloudTrailMetrics`
*   **Metric Name**: `SecurityGroupChangesCount`

---

### (11) Network ACL Changes

Monitors for changes to Network Access Control Lists (NACLs), which control subnet traffic.

*   **Filter Name**: `Network ACL Changes`
*   **Filter Pattern**: `{($.eventName = CreateNetworkAcl) || ($.eventName = CreateNetworkAclEntry) || ($.eventName = DeleteNetworkAcl) || ($.eventName = DeleteNetworkAclEntry) || ($.eventName = ReplaceNetworkAclEntry) || ($.eventName = ReplaceNetworkAclAssociation)}`
*   **Metric Namespace**: `CloudTrailMetrics`
*   **Metric Name**: `NetworkACLChangesCount`

---

### (12) Network Gateway Changes

Detects changes to network gateways (Internet Gateways, Customer Gateways), which can affect network connectivity.

*   **Filter Name**: `Network Gateway Changes`
*   **Filter Pattern**: `{($.eventName = CreateCustomerGateway) || ($.eventName = DeleteCustomerGateway) || ($.eventName = AttachInternetGateway) || ($.eventName = CreateInternetGateway) || ($.eventName = DeleteInternetGateway) || ($.eventName = DetachInternetGateway)}`
*   **Metric Namespace**: `CloudTrailMetrics`
*   **Metric Name**: `NetworkGatewayChangesCount`

---

### (13) Route Table Changes

Monitors for modifications to VPC Route Tables, which dictate network routing.

*   **Filter Name**: `Route Table Changes`
*   **Filter Pattern**: `{($.eventName = CreateRoute) || ($.eventName = CreateRouteTable) || ($.eventName = ReplaceRoute) || ($.eventName = ReplaceRouteTableAssociation) || ($.eventName = DeleteRouteTable) || ($.eventName = DeleteRoute) || ($.eventName = DisassociateRouteTable)}`
*   **Metric Namespace**: `CloudTrailMetrics`
*   **Metric Name**: `RouteTableChangesCount`

---

### (14) VPC Changes

Detects changes to Virtual Private Cloud (VPC) configurations, including creation, deletion, and peering.

*   **Filter Name**: `VPC Changes`
*   **Filter Pattern**: `{($.eventName = CreateVpc) || ($.eventName = DeleteVpc) || ($.eventName = ModifyVpcAttribute) || ($.eventName = AcceptVpcPeeringConnection) || ($.eventName = CreateVpcPeeringConnection) || ($.eventName = DeleteVpcPeeringConnection) || ($.eventName = RejectVpcPeeringConnection) || ($.eventName = AttachClassicLinkVpc) || ($.eventName = DetachClassicLinkVpc) || ($.eventName = DisableVpcClassicLink) || ($.eventName = EnableVpcClassicLink)}`
*   **Metric Namespace**: `CloudTrailMetrics`
*   **Metric Name**: `VPCChangesCount`

---

### (15) Organizations Changes

Monitors for changes within AWS Organizations, such as account creation, policy attachment, or organizational unit modifications.

*   **Filter Name**: `Organizations Changes`
*   **Filter Pattern**: `{($.eventSource = organizations.amazonaws.com) && (($.eventName = AcceptHandshake) || ($.eventName = AttachPolicy) || ($.eventName = CreateAccount) || ($.eventName = CreateOrganizationalUnit) || ($.eventName = CreatePolicy) || ($.eventName = DeclineHandshake) || ($.eventName = DeleteOrganization) || ($.eventName = DeleteOrganizationalUnit) || ($.eventName = DeletePolicy) || ($.eventName = DetachPolicy) || ($.eventName = DisablePolicyType) || ($.eventName = EnablePolicyType) || ($.eventName = InviteAccountToOrganization) || ($.eventName = LeaveOrganization) || ($.eventName = MoveAccount) || ($.eventName = RemoveAccountFromOrganization) || ($.eventName = UpdatePolicy) || ($.eventName = UpdateOrganizationalUnit))}`
*   **Metric Namespace**: `CloudTrailMetrics`
*   **Metric Name**: `OrganizationsChangesCount`

---

## Verification

After creating the metric filters, verify their functionality:

1.  **Check Metric Filter Existence**:
    *   Navigate to the CloudWatch console -> **Logs** -> **Log groups**.
    *   Select your CloudTrail log group.
    *   Go to the **Metric filters** tab. You should see all the metric filters you created listed there.

## Next Steps

Once the metric filters are in place and verified, consider these crucial next steps to fully leverage your security monitoring:

1.  **Create CloudWatch Alarms**:
    *   For each security metric, create a CloudWatch Alarm that triggers when the metric exceeds a certain threshold within a specified period.
    *   Configure these alarms to send notifications to appropriate channels (e.g., SNS topic, email, PagerDuty, Slack via Lambda).
    *   Example: Create an alarm for `UnauthorizedAPICallsCount` that triggers if the count is greater than 0 over a 5-minute period.
2.  **Build CloudWatch Dashboards (Optional)**:
    *   Create custom CloudWatch Dashboards to visualize the metrics generated by these filters. This provides a centralized view of your security posture.
3.  **Automate Remediation (Optional)**:
    *   For critical alarms, consider setting up automated remediation actions using AWS Lambda functions. For example, an alarm for `RootAccountUsageCount` could trigger a Lambda function to notify security teams and potentially disable the root user's access keys (if they exist and are not protected by MFA).
