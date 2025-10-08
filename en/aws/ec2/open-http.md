# AWS / EC2 / Open HTTP

## Quick Info

| | |
|-|-|
| **Plugin Title** | Open HTTP |
| **Cloud** | AWS |
| **Category** | EC2 |
| **Description** | Determine if TCP port 80 for HTTP is open to the public |
| **More Info** | While some ports are required to be open to the public to function properly, more sensitive services such as HTTP should be restricted to known IP addresses. |
| **AWS Link** | http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/authorizing-access-to-an-instance.html |
| **Recommended Action** | Restrict TCP port 80 to known IP addresses |

### Introduction

This guide explains how to secure TCP port 80, used by HTTP, by restricting access to trusted sources only. Allowing public access (e.g., via 0.0.0.0/0 or ::/0) poses serious security risks, including unauthorized access, data breaches and denial-of-service attacks. Remediation involves updating security group rules to limit access on this port strictly to approved IP addresses or networks.

### Detailed Remediation Steps

Follow these granular steps to restrict access to TCP port 80 for HTTP:

1.  **Identify the Affected Security Group**:
    *   Sign in to the AWS Management Console.
    *   Navigate to the Amazon EC2 service.
    *   In the navigation pane, under **Network & Security**, choose **Security Groups**.
    *   Locate the security group(s) having an open HTTP port (TCP 80). Note down the Security Group ID.

2.  **Modify Inbound Rules**:
    *   Select the identified security group by clicking its ID or name.
    *   Choose the **Inbound Rules** tab.
    *   Click **Edit inbound rules**.

3.  **Remove Overly Permissive Rule(s)**:
    *   Carefully review the existing inbound rules.
    *   Locate any rule(s) that allow inbound traffic on port `80` (for either TCP protocol) from `0.0.0.0/0` (for IPv4) or `::/0` (for IPv6).
    *   For each such rule, click the **Delete** button next to it.

4.  **Add Restricted Inbound Rule(s)**:
    *   Click **Add rule**.
    *   **For TCP Port 80**:
        *   For **Type**, choose **Custom TCP**.
        *   For **Port range**, enter `80`.
        *   For **Source**, enter the specific IP address or CIDR block (e.g., `192.0.2.0/24`) that should have access to HTTP. Alternatively, you can select "Custom" and then select a security group ID from the dropdown next to it, if the access is required from other instances within AWS.
        *   (Optional) Add a descriptive comment (e.g., "Allow HTTP TCP access from trusted internal network").
    *   If you have multiple trusted IP ranges or security groups that require access, repeat the "Add rule" process for each.

5.  **Save Changes**:
    *   After adding all necessary restricted rules and removing the permissive ones, click **Save rules**.
    
6.  **Repeat**:
    *   Repeat **steps 2 to 5** for all the security groups found in the step 1.

### Verification

Perform the following steps to verify that the remediation was successful:

1.  **Confirm Security Group Rules**:
    *   After saving the changes, navigate back to the details of the modified security group in the EC2 console.
    *   Review the **Inbound Rules** tab.
    *   Ensure that there are no rules allowing TCP port 80 from `0.0.0.0/0` or `::/0`.
    *   Verify that new rules exist for TCP port 80, and their sources are strictly limited to your known IP addresses/CIDR blocks or specified security groups.

2.  **Test Access from Unauthorized Source**:
    *   From an IP address or instance that is *not* included in the newly configured allowed sources for port 80, attempt to connect to the HTTP instance.
    *   You can use tool like `telnet <HTTP Endpoint> 80`.
    *   Confirm that the connection attempt fails (e.g., connection timed out or refused), indicating that unauthorized access is denied.

3.  **Test Access from Authorized Source**:
    *   From an IP address or instance that *is* included in the newly configured allowed sources for port 80, attempt to connect to the HTTP instance.
    *   Confirm that the connection attempt is successful, indicating that authorized access is granted.

### Next Steps

Consider these additional security measures to further enhance the security posture of your AWS environment:

*   **Regular Security Group Review**: Periodically review all security group rules to ensure they adhere to the principle of least privilege and are still necessary. Remove any outdated or unused rules.
