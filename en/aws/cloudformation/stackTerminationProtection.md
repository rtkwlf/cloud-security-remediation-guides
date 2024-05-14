[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)
# AWS / CloudFormation / CloudFormation Stack Termination Protection Enabled
## Quick Info
| | |
|-|-|
| **Plugin Title** | CloudFormation Stack Termination Protection Enabled |
| **Cloud** | AWS |
| **Category** | CloudFormation |
| **Description** | Ensures that AWS CloudFormation stacks have termination protection enabled. |
| **More Info** | AWS CloudFormation stacks should have termination protection enabled to avoid accidental stack deletion. |
| **AWS Link** | https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-protect-stacks.html |
| **Recommended Action** | Enable termination protection for CloudFormation stack |
## Detailed Remediation Steps
1. Log into the console.
2. Ensure you are in the region with the failing test.
3. Search services and find Cloudformation.![alt text](../../../resources/aws/cloudformation/search-cfn-service.png)
4. Click on Stacks.![alt text](../../../resources/aws/cloudformation/click-stacks.png)
5. Search for the offending stack.![alt text](../../../resources/aws/cloudformation/search-stack.png)
6. In the stack details pane, select Stack actions and then Edit termination protection.
7. CloudFormation displays the Edit termination protection dialog box.
8. Choose Enable or Disable, and then select Save.