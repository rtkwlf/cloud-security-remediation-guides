# AWS / SNS / SNS Topic No HTTP Policy

## Quick Info

| | |
|-|-|
| **Plugin Title** | SNS Topic HTTP Protocol Restriction |
| **Cloud** | AWS |
| **Category** | SNS |
| **Description** | Ensures SNS topics do not allow HTTP protocol |
| **More Info** | SNS topics should be configured to restrict access to the HTTP protocol to prevent unauthorized send or subscribe operations. |
| **AWS Link** | https://docs.aws.amazon.com/sns/latest/dg/sns-using-identity-based-policies.html |
| **Recommended Action** | Modify the topic policy to deny HTTP protocol and only allow secure protocols like HTTPS for send or subscribe operations. |

## Detailed Remediation Steps
1. Log in to the AWS Management Console.
2. Select the "Services" option and search for SNS. </br>
3. In the left navigation panel, select "Topics" under SNS Dashboard. </br>
4. Select the Topic that allows HTTP protocol by clicking on the Topic ARN.</br>
5. In the Topic configuration page, scroll down and click on the "Access policy" tab to review the current policy.</br>
6. Review the policy for statements that allow the HTTP protocol. The plugin specifically checks for "Condition" blocks with "StringEquals" using "SNS:Protocol" (note: case-sensitive). Look for:
   a. Statements with "Effect": "Allow" and "Condition" that includes "SNS:Protocol": "http" (this allows HTTP - should be changed)
   b. Statements with "Effect": "Deny" and "Condition" that includes "SNS:Protocol": "https" (this denies HTTPS - should be removed)
   c. Missing "Condition" blocks that should restrict the protocol to only secure options
7. To remediate the HTTP protocol access, click on the "Edit" button at the top of the page.</br>
8. On the "Edit topic" page, scroll down to "Access policy" and in the "JSON editor", modify the policy to restrict HTTP protocol access. Add or modify the "Condition" block to only allow HTTPS protocol (note: use "SNS:Protocol" with uppercase):</br>
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {
      "AWS": "arn:aws:iam::YOUR-ACCOUNT-ID:root"
    },
    "Action": [
      "SNS:Subscribe",
      "SNS:Receive"
    ],
    "Resource": "arn:aws:sns:REGION:YOUR-ACCOUNT-ID:topic-name",
    "Condition": {
      "StringEquals": {
        "SNS:Protocol": "https"
      }
    }
  }]
}
```
9. If you need to allow multiple secure protocols (excluding HTTP), use this example with "StringEquals" condition:</br>
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {
      "AWS": "arn:aws:iam::YOUR-ACCOUNT-ID:root"
    },
    "Action": "SNS:Subscribe",
    "Resource": "arn:aws:sns:REGION:YOUR-ACCOUNT-ID:topic-name",
    "Condition": {
      "StringEquals": {
        "SNS:Protocol": [
          "https",
          "email",
          "email-json",
          "sqs",
          "lambda"
        ]
      }
    }
  }]
}
```
Note: The list includes secure protocols but explicitly excludes "http".
10. Alternatively, you can deny HTTP protocol explicitly while allowing all other protocols:</br>
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::YOUR-ACCOUNT-ID:root"
      },
      "Action": "SNS:Subscribe",
      "Resource": "arn:aws:sns:REGION:YOUR-ACCOUNT-ID:topic-name"
    },
    {
      "Effect": "Deny",
      "Principal": "*",
      "Action": "SNS:Subscribe",
      "Resource": "arn:aws:sns:REGION:YOUR-ACCOUNT-ID:topic-name",
      "Condition": {
        "StringEquals": {
          "SNS:Protocol": "http"
        }
      }
    }
  ]
}
```
11. Replace `YOUR-ACCOUNT-ID`, `REGION`, and `topic-name` with your actual AWS account ID, region, and topic name.</br>
12. Click on the "Save changes" button at the bottom of the page.</br>
13. Verify the policy by attempting to create an HTTP subscription (it should be denied).</br>
14. Review any existing HTTP subscriptions and migrate them to HTTPS endpoints:
   a. In the Topic details page, click on the "Subscriptions" tab
   b. Identify any subscriptions using "http" protocol
   c. Delete the HTTP subscriptions
   d. Create new subscriptions using "https" protocol with the updated HTTPS endpoints
15. Repeat steps 4-14 for all other SNS Topics that allow HTTP protocol across all regions.</br>