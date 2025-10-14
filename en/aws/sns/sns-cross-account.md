# AWS / SNS / SNS Cross Account

## Quick Info

| | |
|-|-|
| **Plugin Title** | SNS Cross Account |
| **Cloud** | AWS |
| **Category** | SNS |
| **Description** | Ensures SNS policies disallow cross-account access |
| **More Info** | SNS topic policies should be carefully restricted to subscribe or send messages. Topic policies can be used to limit these privileges. |
| **AWS Link** | https://docs.aws.amazon.com/sns/latest/dg/sns-access-policy-use-cases.html |
| **Recommended Action** | Update the SNS policy to prevent access from external accounts. |

## Detailed Remediation Steps
**Note:** This plugin checks for cross-account access in SNS topic policies. The plugin can be configured with the following settings:
- **sns_whitelisted_aws_account_principals**: Comma-separated list of trusted cross-account principals that are allowed (e.g., `arn:aws:iam::123456789012:root,arn:aws:iam::210987654321:root`)
- **sns_whitelist_aws_organization_accounts**: If set to 'true', all accounts in the current AWS organization will be trusted (default: 'false')
- **sns_topic_policy_condition_keys**: Comma-separated list of AWS IAM condition keys that should be allowed (default: `aws:PrincipalArn,aws:PrincipalAccount,aws:PrincipalOrgID,aws:SourceAccount,aws:SourceArn,aws:SourceOwner,sns:Endpoint`)

1. Log in to the AWS Management Console.
2. Select the "Services" option and search for SNS.
3. In the left navigation panel, select "Topics" under SNS Dashboard.
4. Select the Topic that has cross-account access by clicking on the Topic ARN.
5. In the Topic configuration page, scroll down and click on the "Access policy" tab to review the current policy.
6. Review the "Principal" key in the policy. Check if any statements contain AWS account IDs that are not your own account or trusted accounts. Cross-account principals may appear in formats like:
   - "arn:aws:iam::123456789012:root" (different account ID)
   - "arn:aws:iam::123456789012:user/username" (user in different account)
   - "arn:aws:iam::123456789012:role/rolename" (role in different account)
7. Check the "Condition" keys to ensure they properly restrict access. The plugin specifically validates these condition keys:
   - **StringEquals** with **aws:SourceAccount**: Should contain only your account ID or whitelisted account IDs
   - **StringEquals** with **aws:PrincipalAccount**: Should contain only your account ID or whitelisted account IDs
   - **ArnLike** with **aws:PrincipalArn** or **aws:SourceArn**: Should reference only your account or whitelisted accounts

   Avoid conditions that could inadvertently grant broad cross-account access.
8. **Important Note on Wildcards**: If the Principal is set to "*" (wildcard), this is only acceptable when accompanied by a valid condition that restricts access to your account. For example:
   ```json
   {
     "Principal": "*",
     "Condition": {
       "StringEquals": {
         "aws:SourceAccount": "YOUR-ACCOUNT-ID"
       }
     }
   }
   ```
   Without such conditions, wildcard principals grant public access and should be removed.
9. To remediate the cross-account access, click on the "Edit" button at the top of the page.
10. On the "Edit topic" page, scroll down to "Access policy" and in the "JSON editor", modify the policy:
   - Remove any "Principal" entries that reference external AWS accounts (accounts that are not your own)
   - If you have whitelisted/trusted accounts, ensure only those accounts are specified
   - Remove or update "Condition" blocks that allow unintended cross-account access
   - Ensure the policy restricts access to only your account ID or explicitly trusted organization accounts
11. Example of a secure policy that prevents cross-account access:</br>
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {
      "AWS": "arn:aws:iam::YOUR-ACCOUNT-ID:root"
    },
    "Action": [
      "SNS:Publish",
      "SNS:Subscribe"
    ],
    "Resource": "arn:aws:sns:us-east-1:YOUR-ACCOUNT-ID:topic-name"
  }]
}
```
12. Click on "Save changes" button at the bottom of the page.</br>
13. Repeat steps 4-12 for all other SNS Topics that have cross-account access across all regions.</br>
