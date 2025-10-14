# AWS / SNS / SNS Subscription HTTPS Only

## Quick Info

| | |
|-|-|
| **Plugin Title** | SNS Subscription HTTPS Only |
| **Cloud** | AWS |
| **Category** | SNS |
| **Description** | Ensures that Amazon SNS subscriptions are configured to use HTTPS protocol |
| **More Info** | Amazon Simple Notification Service (Amazon SNS) is a managed service that provides message delivery from publishers to subscribers. It is important to verify that SNS subscriptions are configured to use the HTTPS protocol. |
| **AWS Link** | https://docs.aws.amazon.com/sns/latest/dg/sns-http-https-endpoint-as-subscriber.html |
| **Recommended Action** | Create a new SNS subscription using HTTPS protocol. |

## Detailed Remediation Steps
1. Log in to the AWS Management Console.
2. Select the "Services" option and search for SNS. </br>
3. In the left navigation panel, select "Subscriptions" under SNS Dashboard. </br>
4. Review all subscriptions and identify any that are not using "HTTPS" protocol:</br>
   a. Check the "Protocol" column for each subscription</br>
   b. Note subscriptions using HTTP, email, SMS, SQS, Lambda, etc.</br>
   c. Prioritize HTTP endpoints for immediate migration (security risk)</br>
5. For each HTTP endpoint subscription (highest priority):</br>
   a. Note the subscription details (endpoint URL, owner, topic ARN)</br>
   b. Verify the endpoint can support HTTPS with a valid SSL/TLS certificate</br>
   c. Set up HTTPS on the endpoint server with a certificate from a trusted CA</br>
   d. Test HTTPS connectivity: `curl -v https://your-endpoint.com/path`</br>
6. Create a new HTTPS subscription to replace the HTTP subscription:</br>
   a. Click on the Topic ARN associated with the HTTP subscription</br>
   b. In the Topic details page, click "Create subscription"</br>
   c. For "Protocol", select "HTTPS"</br>
   d. For "Endpoint", enter the HTTPS URL (e.g., `https://example.com/sns-webhook`)</br>
   e. Configure any settings to match the original subscription (filter policy, delivery retry, etc.)</br>
   f. Click "Create subscription"</br>
7. Confirm the HTTPS subscription:</br>
   a. Your endpoint will receive a SubscriptionConfirmation POST request</br>
   b. Extract the SubscribeURL from the JSON payload</br>
   c. Make an HTTP GET request to the SubscribeURL to confirm</br>
   d. The subscription status will change from "PendingConfirmation" to "Confirmed"</br>
8. Test the HTTPS subscription:</br>
   a. Go to the Topic details page</br>
   b. Click "Publish message"</br>
   c. Send a test message</br>
   d. Verify your HTTPS endpoint receives and processes the message correctly</br>
   e. Monitor for at least one message delivery cycle to ensure reliability</br>
9. Once confirmed working, delete the old HTTP subscription:</br>
   a. Navigate back to SNS > Subscriptions</br>
   b. Select the old HTTP subscription</br>
   c. Click "Delete"</br>
   d. Confirm the deletion</br>
10. For SQS and Lambda subscriptions (secure alternatives):</br>
   a. These protocols are secure by default (encrypted, no public exposure, IAM-authenticated)</br>
   b. Document these subscriptions as approved exceptions in your security policy</br>
   c. Consider configuring the plugin to exclude these protocols from checks</br>
   d. No migration is required unless your organization requires strict HTTPS-only policy</br>
11. For email, SMS, and application endpoint subscriptions:</br>
   a. Email: Uses TLS encryption when available; document as approved if needed for notifications</br>
   b. SMS: Inherently insecure but often required for critical alerts; document as approved exception</br>
   c. Application endpoints: Used for mobile push notifications; document as approved if required</br>
   d. If these must be converted to HTTPS, set up an HTTPS webhook gateway that forwards to these services</br>
12. To prevent future HTTP subscriptions, update topic policies:</br>
   a. Navigate to SNS > Topics</br>
   b. Select each topic and click "Edit"</br>
   c. Scroll to "Access policy" and add this statement to deny HTTP subscriptions:</br>
```json
{
  "Sid": "DenyHTTPSubscriptions",
  "Effect": "Deny",
  "Principal": "*",
  "Action": "SNS:Subscribe",
  "Resource": "arn:aws:sns:REGION:ACCOUNT-ID:topic-name",
  "Condition": {
    "StringEquals": {
      "SNS:Protocol": "http"
    }
  }
}
```
   d. Replace `REGION`, `ACCOUNT-ID`, and `topic-name` with your actual values</br>
   e. Click "Save changes"</br>
13. Repeat steps 4-12 for all regions:</br>
   a. Click the region selector in the top-right corner</br>
   b. Select the next region (e.g., us-west-1, us-west-2, eu-west-1, etc.)</br>
   c. Navigate to SNS > Subscriptions</br>
   d. Review and remediate subscriptions in that region</br>
   e. Continue until all regions have been checked</br>
14. Verify complete remediation:</br>
   a. Run the security scan again after making changes</br>
   b. Review findings to confirm HTTP subscriptions have been migrated</br>
   c. Document all remaining non-HTTPS subscriptions (SQS, Lambda, email, etc.) as approved exceptions</br>
   d. Test all subscriptions to ensure they function correctly</br>
15. Set up monitoring for future subscriptions:</br>
   a. Create a CloudWatch Events rule for `sns:Subscribe` actions</br>
   b. Configure alerts when new non-HTTPS subscriptions are created</br>
   c. Implement quarterly reviews of all SNS subscriptions</br>
   d. Maintain documentation of all approved protocol exceptions</br>