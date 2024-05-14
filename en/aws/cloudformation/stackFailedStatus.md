[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)
# AWS / CloudFormation / CloudFormation Stack Failed Status
## Quick Info
| | |
|-|-|
| **Plugin Title** | CloudFormation Stack Failed Status |
| **Cloud** | AWS |
| **Category** | CloudFormation |
| **Description** | Ensures that AWS CloudFormation stacks are not in Failed mode for more than the maximum failure limit hours. |
| **More Info** | AWS CloudFormation stacks should not be in failed mode to avoid application downtime. |
| **AWS Link** | https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-view-stack-data-resources.html |
| **Recommended Action** | Remove or redeploy the CloudFormation failed stack. |
## Detailed Remediation Steps
1. Log into the console.
2. Ensure you are in the region with the failing test.
3. Search services and find Cloudformation.![alt text](../../../resources/aws/cloudformation/search-cfn-service.png)
4. Click on Stacks.![alt text](../../../resources/aws/cloudformation/click-stacks.png)
5. Search for the offending stack.![alt text](../../../resources/aws/cloudformation/search-stack.png)
6. Use stack status codes to troubleshoot the failing stack. https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-view-stack-data-resources.html 
7. Either delete update or rollback failed stack.