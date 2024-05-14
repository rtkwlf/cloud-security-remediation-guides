[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)
# AWS / CloudFormation / CloudFormation Stack SNS Notifications
## Quick Info
| | |
|-|-|
| **Plugin Title** | CloudFormation Stack SNS Notifications |
| **Cloud** | AWS |
| **Category** | CloudFormation |
| **Description** | Ensures that AWS CloudFormation stacks have SNS topic associated. |
| **More Info** | AWS CloudFormation stacks should have SNS topic associated to ensure stack events monitoring. |
| **AWS Link** | https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-view-stack-data-resources.html |
| **Recommended Action** | Associate an Amazon SNS topic to all CloudFormation stacks |
## Detailed Remediation Steps
1. Log into the console.
2. Ensure you are in the region with the failing test.
3. Search services and find Cloudformation.![alt text](../../../resources/aws/cloudformation/search-cfn-service.png)
4. Click on Stacks.![alt text](../../../resources/aws/cloudformation/click-stacks.png)
5. Search for the offending stack.![alt text](../../../resources/aws/cloudformation/search-stack.png)
6. In the stack details pane, choose Update.
7. Select Use current template
8. Click on notification options and a SNS topic where notifications will be sent on the stack. 
9. When you are satisfied with your changes, choose Update stack.