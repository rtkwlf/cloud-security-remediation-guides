# AWS / EC2 / Internet Gateway In VPC

## Quick Info

| | |
|-|-|
| **Plugin Title** | Internet Gateway In VPC |
| **Cloud** | AWS |
| **Category** | EC2 |
| **Description** | Ensure Internet Gateways are associated with at least one available VPC. |
| **More Info** | Internet Gateways allow communication between instances in VPC and the internet. They provide a target in VPC route tables for internet-routable traffic and also perform network address translation (NAT) for instances that have been assigned public IPv4 addresses. Make sure they are always associated with a VPC to meet security and compliance requirements within your organization. |
| **AWS Link** | https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html |
| **Recommended Action** | Ensure Internet Gateways have VPC attached to them. |

### Introduction

An Internet Gateway (IGW) is a crucial component for enabling communication between your Virtual Private Cloud (VPC) and the internet. It is a horizontally scaled, redundant, and highly available VPC component that supports both IPv4 and IPv6 traffic. The IGW serves as a target in your VPC route tables for internet-routable traffic and performs network address translation (NAT) for IPv4 traffic.

The absence of a VPC attachment to an Internet Gateway means that the gateway is not actively serving its purpose, potentially leaving resources within your VPC unable to communicate with the internet or vice versa. Ensuring that all Internet Gateways are associated with at least one available VPC is essential for maintaining proper network connectivity, security, and compliance within your AWS environment.

### Prerequisites

Before proceeding with the remediation steps, ensure the following:

*   **AWS Account and Permissions**: You have an active AWS account with appropriate IAM permissions to manage Amazon VPC resources, including Internet Gateways and VPCs.
*   **Existing VPC**: You have an existing Amazon VPC to which you intend to attach the Internet Gateway. If not, you will need to create one.

### Detailed Remediation Steps

Follow these steps to attach an Internet Gateway to an available VPC using the AWS Management Console:

1.  **Open the Amazon VPC console**: Navigate to [https://console.aws.amazon.com/vpc/](https://console.aws.amazon.com/vpc/).
2.  **Select Internet Gateways**: In the navigation pane, choose **Internet gateways**.
3.  **Select the unattached Internet Gateway**: Locate and select the check box next to the Internet Gateway that is not associated with a VPC.
4.  **Initiate attachment**: Choose **Actions**, and then select **Attach to VPC**.
5.  **Choose a VPC**: From the list of available VPCs, select the VPC to which you want to attach the Internet Gateway.
6.  **Confirm attachment**: Choose **Attach internet gateway**.

### Verification

To verify that the Internet Gateway is successfully attached to a VPC:

1.  **Open the Amazon VPC console**: Navigate to [https://console.aws.amazon.com/vpc/](https://console.aws.amazon.com/vpc/).
2.  **Select Internet Gateways**: In the navigation pane, choose **Internet gateways**.
3.  **Check the Internet Gateway details**: Locate the Internet Gateway you just modified. In the details pane or the main table, verify that the **State** is `attached` and that a **VPC ID** is listed under its attachments. To verify the state of the attached VPC, Click on the VPC ID link and on the VPC details page, check that the State column shows `available`.
