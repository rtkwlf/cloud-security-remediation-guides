[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)
# AWS / CloudFormation / AWS CloudFormation In Use
## Quick Info
| | |
|-|-|
| **Plugin Title** | AWS CloudFormation In Use |
| **Cloud** | AWS |
| **Category** | CloudFormation |
| **Description** | Ensure that Amazon CloudFormation service is in use within your AWS account to automate your infrastructure management and deployment. |
| **More Info** | AWS CloudFormation is a service that helps you model and set up your AWS resources so that you can spend less time managing those resources and more time focusing on your applications that run in AWS. ' |
| **AWS Link** | https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html |
| **Recommended Action** | Check if CloudFormation is in use or not by observing the stacks |
## Detailed Remediation Steps
1. Log into the console.
2. Ensure you are in the region with the failing test.
3. Search services and find Cloudformation.![alt text](../../../resources/aws/cloudformation/search-cfn-service.png)
4. Click on Stacks.![alt text](../../../resources/aws/cloudformation/click-stacks.png)