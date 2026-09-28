
# AWS / AutoScaling / Auto Scaling Group Cooldown Period

## Quick Info

| | |
|-|-|
| **Plugin Title** | Auto Scaling Group Cooldown Period |
| **Cloud** | AWS |
| **Category** | AutoScaling |
| **Description** | Ensure that your AWS Auto Scaling Groups are configured to use a cool down period. |
| **More Info** | A scaling cool down helps you prevent your Auto Scaling group from launching or terminating additional instances before the effects of previous activities are visible. |
| **AWS Link** | https://docs.aws.amazon.com/autoscaling/ec2/userguide/Cooldown.html |
| **Recommended Action** | Implement proper cool down period for Auto Scaling groups to temporarily suspend any scaling actions. |

## Detailed Remediation Steps
1. Log in to the AWS Management Console.
2. Select the "Services" option and search for EC2. </br> <img src="/resources/aws/autoscaling/auto-scaling-group-cooldown-period/step2.png"/>
3. In the EC2 Management console, scroll down and click on the "Auto Scaling groups" at the bottom.</br> <img src="/resources/aws/autoscaling/auto-scaling-group-cooldown-period/step3.png"/>
4. On the "Auto Scaling groups" page, select the Auto Scaling group flagged in the scan results.</br> <img src="/resources/aws/autoscaling/auto-scaling-group-cooldown-period/step4.png"/>
5. Select the "Details" tab and scroll down to the "Advanced configurations" section, then check the value for "Default cooldown".</br> <img src="/resources/aws/autoscaling/auto-scaling-group-cooldown-period/step5.png"/>
6. If the "Default cooldown" value is not set, click "Edit".</br> <img src="/resources/aws/autoscaling/auto-scaling-group-cooldown-period/step6.png"/>
7. Under "Advanced configurations", enter a cooldown period, in seconds, for the "Default cooldown" field (e.g. 300).</br> <img src="/resources/aws/autoscaling/auto-scaling-group-cooldown-period/step7.png"/>
8. Scroll down to the end of the page and click "Update" to save the changes.</br> <img src="/resources/aws/autoscaling/auto-scaling-group-cooldown-period/step8.png"/>
9. Go to the "Details" tab and confirm the "Default cooldown" value has been updated.</br> <img src="/resources/aws/autoscaling/auto-scaling-group-cooldown-period/step9.png"/>
10. Repeat steps number 4 - 9 to check other Auto Scaling groups in the account.
