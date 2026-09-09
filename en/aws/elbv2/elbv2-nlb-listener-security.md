# AWS / ELBv2 / ELBv2 NLB Listener Security

## Quick Info

| | |
|-|-|
| **Plugin Title** | ELBv2 NLB Listener Security |
| **Cloud** | AWS |
| **Category** | ELBv2 |
| **Description** | Ensures that AWS Network Load Balancers have secured listener configured. |
| **More Info** | AWS Network Load Balancer should have TLS protocol listener configured to terminate TLS traffic. |
| **AWS Link** | https://docs.amazonaws.cn/en_us/elasticloadbalancing/latest/network/create-tls-listener.html |
| **Recommended Action** | Attach TLS listener to AWS Network Load Balancer |

## Introduction

A Network Load Balancer listener accepts incoming traffic on a configured port and protocol. Without a TLS listener, traffic may not be encrypted in transit and the Network Load Balancer cannot terminate TLS connections.

## Prerequisites

- Access to the affected AWS account.
- Permission to modify Network Load Balancers and listeners.
- An ACM certificate for the listener domain.
- A target group for the listener’s default forwarding action.

## Remediation Steps

#### Configure a TLS listener

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/).
2. Select the AWS Region containing the affected Network Load Balancer.
3. Select **Load Balancers** under **Load Balancing**.
4. Select the affected Network Load Balancer.  </br> <img src="/resources/aws/elbv2/elbv2-nlb-listener-security/step1.png"/>
5. Select the **Listeners and rules** tab. </br> <img src="/resources/aws/elbv2/elbv2-nlb-listener-security/step2.png"/>
6. Click **Add listener**. </br> <img src="/resources/aws/elbv2/elbv2-nlb-listener-security/step2.png"/>
7. Select **TLS** for **Protocol**.
8. Enter the listener port in **Port**.
9. Select **Forward to target groups** for **Default action**. </br> <img src="/resources/aws/elbv2/elbv2-nlb-listener-security/step3.png"/>
10. Select the target group that should receive the traffic.
11. Select the ACM certificate under **Default SSL/TLS certificate**.
12. Select **Add listener**.  </br> <img src="/resources/aws/elbv2/elbv2-nlb-listener-security/step4.png"/>

## Verification Steps

#### Confirm the TLS listener

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/).
2. Select the AWS Region containing the affected Network Load Balancer.
3. Select **Load Balancers** under **Load Balancing**.
4. Select the affected Network Load Balancer.
5. Select the **Listeners and rules** tab.
6. Confirm that a listener uses the **TLS** protocol.
7. Confirm that the TLS listener has the required port, certificate, and forwarding action configured.
