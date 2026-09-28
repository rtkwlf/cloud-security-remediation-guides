
# AWS / AutoScaling / Auto Scaling Group Missing ELB

## Quick Info   

| | |
|-|-|
| **Plugin Title** | Auto Scaling Group Missing ELB |
| **Cloud** | AWS |
| **Category** | AutoScaling |
| **Description** | Ensures all Auto Scaling groups are referencing active load balancers. |
| **More Info** | Each Auto Scaling group with a load balancer configured should reference an active ELB. |
| **AWS Link** | https://docs.aws.amazon.com/autoscaling/ec2/userguide/attach-load-balancer-asg.html |
| **Recommended Action** | Ensure that the Auto Scaling group load balancer has not been deleted. If so, remove it from the ASG. |

## Detailed Remediation Steps
1. Log in to the AWS Management Console.
2. Select the "Services" option and search for EC2. </br> <img src="/resources/aws/autoscaling/auto-scaling-group-missing-elb/step2.png"/>
3. In the EC2 Management console, scroll down and click on the "Auto Scaling groups" at the bottom.</br> <img src="/resources/aws/autoscaling/auto-scaling-group-missing-elb/step3.png"/>
4. On the "Auto Scaling groups" page, select the Auto Scaling group flagged in the scan results.</br> <img src="/resources/aws/autoscaling/auto-scaling-group-missing-elb/step4.png"/>
5. Select the "Integrations" tab and check the "Load balancing" section to view the load balancer(s) currently referenced by the Auto Scaling group.</br> <img src="/resources/aws/autoscaling/auto-scaling-group-missing-elb/step5.png"/>
6. Navigate to the EC2 console using the link https://console.aws.amazon.com/ec2/ and select "Load Balancers" from the left navigation pane.</br> <img src="/resources/aws/autoscaling/auto-scaling-group-missing-elb/step6.png"/>
7. Confirm whether the load balancer name(s) referenced by the Auto Scaling group are present in this list. If a referenced load balancer is not present, it has been deleted and is no longer active.</br> <img src="/resources/aws/autoscaling/auto-scaling-group-missing-elb/step7.png"/>
8. Return to the "Integrations" tab and click "Edit" next to the "Load balancing" section.</br> <img src="/resources/aws/autoscaling/auto-scaling-group-missing-elb/step8.png"/>
9. Remove the reference to the deleted/inactive load balancer.
10. Click "Update" to save the changes.</br> <img src="/resources/aws/autoscaling/auto-scaling-group-missing-elb/step10.png"/>
11. Repeat steps number 4 - 10 to check other Auto Scaling groups in the account.
