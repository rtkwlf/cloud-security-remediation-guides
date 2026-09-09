## Introduction

A load balancer distributes incoming traffic across registered EC2 targets. If fewer than two EC2 targets are healthy, the service may lack fault tolerance and become unavailable when one target fails.

## Prerequisites

- Access to the affected AWS account.
- Permission to view and modify load balancers, target groups, and registered targets.
- At least two suitable EC2 instances available for registration.

## Remediation Steps

#### Register and restore healthy targets

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/).
2. Select the AWS Region containing the affected load balancer.
3. Select **Target Groups** under **Load Balancing**.
4. Select a target group associated with the affected load balancer. </br> <img src="/resources/aws/elbv2/elbv2-minimum-number-of-ec2-target-instances/step1.png"/>
5. Select the **Targets** tab.
6. Review the **Health status** and **Health status details** for each registered target. </br> <img src="/resources/aws/elbv2/elbv2-minimum-number-of-ec2-target-instances/step2.png"/>
7. Take the appropriate corrective action based on the **Health status details** for each target that is not `healthy`.
8. Select **Register targets** to add an available EC2 instance if fewer than two targets are registered. </br> <img src="/resources/aws/elbv2/elbv2-minimum-number-of-ec2-target-instances/step3.png"/>
9. Select the required EC2 instance.
10. Specify the target port.
11. Click **Include as pending below**.
12. Click **Register pending targets**. </br> <img src="/resources/aws/elbv2/elbv2-minimum-number-of-ec2-target-instances/step4.png"/>
13. Repeat these steps for the affected target groups until at least two targets are healthy.

## Verification Steps

#### Confirm healthy targets

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/).
2. Select the AWS Region containing the affected load balancer.
3. Select **Target Groups** under **Load Balancing**.
4. Select a target group associated with the affected load balancer.
5. Select the **Targets** tab.
6. Confirm that at least two targets have a **Health status** of `healthy`.
7. Repeat the verification for each target group associated with the affected load balancer.
