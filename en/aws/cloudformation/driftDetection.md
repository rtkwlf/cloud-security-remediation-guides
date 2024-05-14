[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)
# AWS / CloudFormation / CloudFormation Drift Detection
## Quick Info
| | |
|-|-|
| **Plugin Title** | CloudFormation Drift Detection |
| **Cloud** | AWS |
| **Category** | CloudFormation |
| **Description** | Ensures that AWS CloudFormation stacks are not in a drifted state. |
| **More Info** | AWS CloudFormation stack should not be in drifted state to ensure that stack template is aligned with the resources. |
| **AWS Link** | https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/resource-import-resolve-drift.html |
| **Recommended Action** | Resolve CloudFormation stack drift by importing drifted resource back to the stack. |
## Detailed Remediation Steps
1. Log into the console.
2. Ensure you are in the region with the failing test.
3. Search services and find Cloudformation.![alt text](../../../resources/aws/cloudformation/search-cfn-service.png)
4. Click on Stacks.![alt text](../../../resources/aws/cloudformation/click-stacks.png)
5. Search for the offending stack.![alt text](../../../resources/aws/cloudformation/search-stack.png)
6. Add a DeletionPolicy attribute, set to Retain, to the resource. This ensures the existing resource is retained rather than deleted when it's removed from the stack. https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/resource-import-resolve-drift.html#resource-import-resolve-drift-console-step-01-update-stack
7. Remove the resource from the template and run a stack update operation. This removes the resource from the stack, but doesn't delete it. https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/resource-import-resolve-drift.html#resource-import-resolve-drift-console-step-02-remove-drift
8. Describe the resource’s actual state in the stack template, and then import the existing resource back into the stack. This adds the resource back into the stack and resolves the property differences that were causing the drift results. https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/resource-import-resolve-drift.html#resource-import-resolve-drift-console-step-03-update-template