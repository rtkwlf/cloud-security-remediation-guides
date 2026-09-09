# AWS / ELBv2 / ELBv2 Unhealthy Instances

## Quick Info

| | |
|-|-|
| **Plugin Title** | ELBv2 Unhealthy Instances |
| **Cloud** | AWS |
| **Category** | ELBv2 |
| **Description** | Ensures that ELBv2 have healthy instances attached |
| **More Info** | ELBs should have healthy instances to ensure proper load balancing and availability. Unhealthy instances can result in degraded performance or service disruptions. |
| **AWS Link** | https://docs.aws.amazon.com/elasticloadbalancing/latest/application/target-group-health-checks.html |
| **Recommended Action** | Investigate and resolve the health issues with the instances attached to the ELB. |

## Introduction

A load balancer distributes incoming traffic across registered targets. These targets should remain healthy to ensure successful traffic routing and service availability. 

## Remediation Steps

#### Diagnose and remediate unhealthy targets

| Target type | Console location |
|---|---|
| Application Load Balancer target | **EC2 console** → **Load Balancing** → **Target Groups** |
| Network Load Balancer target | **EC2 console** → **Load Balancing** → **Target Groups** |

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/).
2. Select **Target Groups** under **Load Balancing**.
3. Select a specific **Target group**.  </br> <img src="/resources/aws/elbv2/elbv2-unhealthy-instances/step1.png"/>
4. Select the **Targets** tab.
5. Review the **Health status** and **Health status details** for each target.  </br> <img src="/resources/aws/elbv2/elbv2-unhealthy-instances/step2.png"/>
6. Take corrective action based on the displayed **Health status details** for each target whose status is not `healthy`.

## Verification Steps

#### Confirm target health

| Target type | Expected result |
|---|---|
| Application Load Balancer target | **Status** displays `healthy` |
| Network Load Balancer target | **Status** displays `healthy` |

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/).
2. Select **Target Groups** under **Load Balancing**.
3. Select the remediated **Target group**.
4. Select the **Targets** tab.
5. Confirm every registered target displays `healthy` in the **Health Status** column.