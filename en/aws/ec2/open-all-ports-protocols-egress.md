# AWS / EC2 / Open All Ports Protocols Egress
## Quick Info

| | |
|-|-|
| **Plugin Title** | Open All Ports Protocols Egress |
| **Cloud** | AWS |
| **Category** | EC2 |
| **Description** | Determine if security group has all outbound ports or protocols open to the public |
| **More Info** | Security groups should be created on a per-service basis and avoid allowing all ports or protocols in order to implement the Principle of Least Privilege (POLP) and reduce the attack surface. |
| **AWS Link** | http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/authorizing-access-to-an-instance.html |
| **Recommended Action** | Modify the security group to restrict access to only those IP addresses and/or IP ranges that require it. |

### Introduction

AWS Security Groups act as virtual firewalls for your EC2 instances and other associated resources, controlling both inbound and outbound traffic. Identify security groups with overly permissive egress rules, specifically allowing all outbound ports and protocols to the public internet (`0.0.0.0/0` for IPv4 and `::/0` for IPv6).

This configuration violates the Principle of Least Privilege and significantly increases the attack surface of your resources. It allows any traffic to leave your instances, which could be exploited by malicious actors to exfiltrate data, launch attacks, or communicate with unauthorized external services.

This guide provides detailed steps to modify the affected security group, restricting outbound access to only the necessary IP addresses and port ranges, thereby enhancing your AWS environment's security posture.

### Prerequisites

Before proceeding with the remediation steps, ensure you have the following:

*   **AWS Account Access**: You must have an AWS account with appropriate permissions to modify Amazon EC2 security groups.
*   **IAM Permissions**: Your IAM user or role must have permissions to perform actions such as `ec2:RevokeSecurityGroupEgress` and `ec2:AuthorizeSecurityGroupEgress`. For comprehensive management, you might also need `ec2:DescribeSecurityGroups`.

### Detailed Remediation Steps

1.  **Open the Amazon EC2 console**: Navigate to [https://console.aws.amazon.com/ec2/](https://console.aws.amazon.com/ec2/).
2.  **Navigate to Security Groups**: In the navigation pane, choose **Security groups**.
3.  **Select the Security Group**: Locate and select the security group having overly permissive egress rules.
4.  **Edit Outbound Rules**:
    *   With the security group selected, choose the **Outbound rules** tab in the details pane.
    *   Choose **Edit outbound rules**.
5.  **Remove Overly Permissive Rules**:
    *   Identify any rules that have `0.0.0.0/0` (for IPv4) or `::/0` (for IPv6) as the destination and allow "All traffic" or "All protocols" or a very broad port range.
    *   For each such rule, choose the **Delete** button next to it.
6.  **Add Specific Outbound Rules**:
    *   Choose **Add rule**.
    *   For each legitimate outbound traffic requirement identified in the Prerequisites, configure a new rule:
        *   **Type**: Select the appropriate service type (e.g., HTTP, HTTPS, SSH) or choose **Custom TCP**, **Custom UDP**, or **Custom ICMP** for specific protocols and port ranges.
        *   **Protocol**: This will be automatically populated for predefined types. For custom types, select the protocol (e.g., TCP, UDP, ICMP).
        *   **Port range**: For TCP/UDP, specify the exact port or a range of ports (e.g., `80`, `443`, `1024-1034`). For ICMP, choose the ICMP type and code if applicable.
        *   **Destination**: Specify the required destination. This should be a specific IPv4 CIDR block (e.g., `203.0.113.5/32`), an IPv6 CIDR block, another security group ID, or a prefix list ID. **Avoid `Anywhere-IPv4 (0.0.0.0/0)` or `Anywhere-IPv6 (::/0)` unless absolutely necessary and justified.**
        *   **Description (Optional)**: Add a descriptive note for the rule (e.g., "Allow outbound HTTPS to payment gateway").
    *   Repeat this step for all necessary outbound connections.
7.  **Save Rules**: After adding all the necessary specific rules and deleting the broad rules, choose **Save rules**.


        
