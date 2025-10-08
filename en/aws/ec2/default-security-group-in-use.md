# AWS / EC2 / Default Security Group In Use

## Quick Info

| | |
|-|-|
| **Plugin Title** | Default Security Group In Use |
| **Cloud** | AWS |
| **Category** | EC2 |
| **Description** | Ensure that AWS EC2 Instances are not associated with default security group. |
| **More Info** | The default security group allows all traffic inbound and outbound, which can make your resources vulnerable to attacks. Ensure that the Amazon EC2 instances are not associated with the default security groups. |
| **AWS Link** | http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html#default-security-group |
| **Recommended Action** | Modify EC2 instances and change security group. |

### Introduction

This guide provides detailed steps to remediate the issue of Amazon EC2 instances being associated with the default security group. The default security group, by its nature, often allows all inbound and outbound traffic, which can expose your EC2 instances to unnecessary risks and potential attacks. By following this guide, you will learn how to disassociate EC2 instances from the default security group and associate them with more restrictive, purpose-built security groups, thereby enhancing your cloud security posture.

### Detailed Remediation Steps

Follow these steps to change the security groups associated with your EC2 instance. These steps will guide you through creating a new security group (if needed) and then associating it with your instance while removing the default security group.

1.  **Create a New Security Group (if necessary)**:
    *   Open the Amazon EC2 console at [https://console.aws.amazon.com/ec2/](https://console.aws.amazon.com/ec2/).
    *   In the navigation pane, under **Network & Security**, choose **Security Groups**.
    *   Choose **Create security group**.
    *   Provide a **Security group name** (e.g., `my-web-server-sg`) and a **Description**.
    *   Select the appropriate **VPC**.
    *   Add **Inbound rules** and **Outbound rules** based on the specific requirements of your EC2 instance. For example, for a web server, you might add an inbound rule for HTTP (port 80) and HTTPS (port 443) from `0.0.0.0/0` (or a more restrictive IP range) and an inbound rule for SSH (port 22) from your administrative IP address.
    *   Choose **Create security group**.

2.  **Change Security Groups for the Instance**:
    *   In the navigation pane, choose **Instances**.
    *   Select the EC2 instance that is associated with the default security group.
    *   Choose **Actions**, then **Security**, and then **Change security groups**.
    *   In the **Associated security groups** section:
        *   To remove the default security group, choose **Remove** next to the security group named `default`.
        *   To add your new, purpose-built security group, select it from the dropdown list and choose **Add security group**.
    *   Choose **Save**.

        
