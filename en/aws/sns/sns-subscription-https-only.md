# AWS / SNS / SNS Subscription HTTPS Only

## Quick Info

| | |
|-|-|
| **Plugin Title** | SNS Subscription HTTPS Only |
| **Cloud** | AWS |
| **Category** | SNS |
| **Description** | Ensures that Amazon SNS HTTP/HTTPS subscriptions are configured to use HTTPS protocol instead of HTTP |
| **More Info** | Amazon Simple Notification Service (Amazon SNS) is a managed service that provides message delivery from publishers to subscribers. For HTTP/HTTPS endpoint subscriptions, it is important to use the secure HTTPS protocol instead of unencrypted HTTP to protect data in transit. Other subscription protocols like email, SQS, Lambda, etc. are not affected by this check. |
| **AWS Link** | https://docs.aws.amazon.com/sns/latest/dg/sns-http-https-endpoint-as-subscriber.html |
| **Recommended Action** | Replace HTTP subscriptions with HTTPS subscriptions to ensure secure data transmission. |

## Detailed Remediation Steps

1. Log in to the AWS Management Console.
2. Select the "Services" option and search for SNS. </br>
3. In the left navigation panel, select "Topics" under SNS Dashboard. </br>
4. Select a Topic that has subscriptions by clicking on the Topic ARN. </br>
5. In the Topic details page, click on the "Subscriptions" tab to view all subscriptions associated with this topic. </br>
6. Review the "Protocol" column for each subscription. Identify any subscriptions using the "HTTP" protocol. </br>
7. For each HTTP subscription that needs to be migrated to HTTPS:</br>
   a. Note the subscription details (endpoint URL, owner, etc.)</br>
   b. Ensure the endpoint supports HTTPS and has a valid SSL/TLS certificate</br>
   c. Update the endpoint application to handle HTTPS requests if needed</br>
8. To replace an HTTP subscription with HTTPS, first select the HTTP subscription by clicking the checkbox next to it. </br>
9. Click the "Delete" button at the top of the subscriptions list to remove the insecure HTTP subscription. </br>
10. Confirm the deletion when prompted. </br>
11. To create a new HTTPS subscription, click the "Create subscription" button. </br>
12. On the "Create subscription" page:</br>
   a. The "Topic ARN" should already be pre-filled with the current topic</br>
   b. For "Protocol", select "HTTPS" from the dropdown menu</br>
   c. For "Endpoint", enter the HTTPS URL of your endpoint (e.g., `https://example.com/sns-endpoint`)</br>
   d. Ensure the URL starts with `https://` not `http://`</br>
13. (Optional) Configure additional settings:</br>
   a. Enable "Raw message delivery" if your endpoint expects raw messages</br>
   b. Configure "Delivery retry policy" if you need custom retry behavior</br>
   c. Set up "Filter policy" if you want to filter messages sent to this endpoint</br>
14. Click the "Create subscription" button at the bottom of the page. </br>
15. The subscription will be created in a "PendingConfirmation" state. AWS will send a confirmation request to your HTTPS endpoint. </br>
16. Confirm the subscription:</br>
   a. Your endpoint should receive a POST request with a JSON payload containing a "SubscribeURL"</br>
   b. Your application should make an HTTP GET request to this SubscribeURL to confirm the subscription</br>
   c. Alternatively, you can manually confirm by clicking the "Request confirmation" link in the AWS console</br>
17. Once confirmed, the subscription status will change from "PendingConfirmation" to "Confirmed". </br>
18. Verify that your HTTPS endpoint is receiving messages correctly by testing with a sample publish:</br>
   a. Go back to the Topic details page</br>
   b. Click "Publish message"</br>
   c. Enter a test message and click "Publish message"</br>
   d. Verify your HTTPS endpoint receives the message</br>
19. Repeat steps 4-18 for all other topics that have HTTP subscriptions across all regions. </br>
