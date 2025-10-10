# AWS / EC2 / Unrestricted Network ACL Inbound Traffic

## Quick Info

| | |
|-|-|
| **Plugin Title** | Unrestricted Network ACL Inbound Traffic |
| **Cloud** | AWS |
| **Category** | EC2 |
| **Description** | Ensures that no Amazon Network ACL allows inbound/ingress traffic to remote administration ports. |
| **More Info** | Amazon Network ACL should not allow inbound/ingress traffic to remote administration ports to avoid unauthorized access at the subnet level. |
| **AWS Link** | https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html |
| **Recommended Action** | Update Network ACL to allow inbound/ingress traffic to specific port ranges only |

### Introduction

Network Access Control Lists (NACLs) act as a firewall for subnets, controlling both inbound and outbound traffic at the subnet level. Each rule has a number, and rules are evaluated in order, starting with the lowest number. As soon as a rule matches traffic, it's applied, regardless of any higher-numbered rules that might contradict it.

Identify Network ACLs that allow unrestricted inbound traffic (`0.0.0.0/0`) to remote administration ports. This configuration poses a significant security risk as it exposes services running on instances within the associated subnets to the entire internet, potentially leading to unauthorized access, brute-force attacks, and other security vulnerabilities.

This guide provides detailed steps to update your Network ACLs to allow inbound traffic only to specific, necessary port ranges and from restricted source IP addresses, thereby enhancing your AWS environment's security posture.

### Prerequisites

Before proceeding with the remediation steps, ensure you have the following:

1.  **AWS Account Access**: Sufficient permissions to view and modify Network ACLs within your AWS account. This typically requires permissions related to Amazon EC2 and VPC.
2.  **Identify Affected Network ACLs**: Identify and make a list of Network ACL IDs having unrestricted inbound traffic.
3.  **Understand Application Requirements**: A clear understanding of the specific inbound protocols, port ranges, and source IP addresses (CIDR blocks) that are legitimately required for the applications and services running in the subnets associated with the affected Network ACLs. This is crucial to avoid disrupting legitimate traffic. For example, if you have a web server, you might need to allow inbound HTTP (port 80) and HTTPS (port 443) traffic from `0.0.0.0/0`. However, for SSH (port 22) or RDP (port 3389), you should restrict the source to known administrative IP ranges.

### Detailed Remediation Steps

Follow these steps to update your Network ACLs to restrict inbound traffic to specific port ranges and source CIDR blocks.

1.  **Access the Amazon VPC Console**:
    *   Sign in to the AWS Management Console.
    *   Navigate to the Amazon VPC console.

2.  **Locate the Affected Network ACL**:
    *   In the navigation pane, under **Security**, choose **Network ACLs**.
    *   Identify the Network ACLs having unrestricted inbound traffic.

3.  **Review Existing Inbound Rules**:
    *   Select an affected Network ACL.
    *   In the **Inbound Rules** tab, review the existing rules. Pay close attention to rules with:
        *   **Rule Action**: `ALLOW`
        *   **Source**: `0.0.0.0/0` (or a similarly broad CIDR block)
        *   **Port Range**: A port range that includes remote administration ports (e.g., 22, 3389) or other ports that should not be publicly accessible.

4.  **Determine Necessary Inbound Rules**:
    *   Based on your application requirements (identified in the Prerequisites), determine the precise inbound rules needed. For each required rule, specify:
        *   **Rule Number**: Choose a rule number that is lower than any existing `DENY` rules you wish to override, or higher than existing `ALLOW` rules if you want them to take precedence. Remember, rules are evaluated from lowest to highest.
        *   **Type/Protocol**: The type of traffic (e.g., SSH, HTTP, HTTPS) or the specific protocol number.
        *   **Port Range**: The specific port or range of ports (e.g., `22`, `3389`).
        *   **Source**: The specific CIDR block(s) from which the traffic is allowed (e.g., your office IP range for SSH, `0.0.0.0/0` for public web traffic if appropriate).
        *   **Allow/Deny**: `Allow`.

5.  **Modify Network ACL Inbound Rules**:

    *   **Option A: Modify an existing unrestricted rule (if applicable and desired).**
        *   Select the unrestricted inbound rule you wish to modify.
        *   Choose **Edit inbound rules**.
        *   Update the **Source** to a more restrictive CIDR block (e.g., your administrative IP range instead of `0.0.0.0/0`).
        *   Update the **Port Range** to be more specific if it's currently too broad.
        *   Choose **Save changes**.

    *   **Option B: Delete unrestricted rules and add new, specific rules.** This is generally the recommended approach for clarity and security.
        *   **Delete the unrestricted inbound rule(s):**
            *   Select the inbound rule(s) that allow unrestricted access (`0.0.0.0/0`) to remote administration ports.
            *   Choose **Edit inbound rules**.
            *   Select **Remove** next to the rule(s) you want to delete.
            *   Choose **Save changes**.
        *   **Add new, specific inbound rules:**
            *   Choose **Edit inbound rules**.
            *   Choose **Add new rule**.
            *   Enter a **Rule number** (e.g., 100).
            *   Select the **Type** (e.g., SSH, HTTP, HTTPS, Custom TCP Rule).
            *   Specify the **Port Range** (e.g., `22`, `80`, `443`, `3389`, `53`).
            *   Enter the **Source** CIDR block (e.g., `203.0.113.0/24` for your administrative network, or `0.0.0.0/0` only if absolutely necessary for public services like web servers).
            *   **Allow/Deny** is selected for the **Rule action** as per the need.
            *   Repeat for all necessary inbound rules.
            *   Choose **Save changes**.

    *   **Example for common ports:**
        *   **SSH (Port 22):**
            *   Rule Number: `100`
            *   Type: `SSH`
            *   Protocol: `TCP`
            *   Port Range: `22`
            *   Source: `your_admin_ip_range/32` (e.g., `203.0.113.5/32`) or `your_office_network_cidr/24`
            *   Allow/Deny: `Allow`
        *   **RDP (Port 3389):**
            *   Rule Number: `110`
            *   Type: `RDP`
            *   Protocol: `TCP`
            *   Port Range: `3389`
            *   Source: `your_admin_ip_range/32`
            *   Allow/Deny: `Deny`

### Verification

After applying the remediation steps, verify that the Network ACL rules have been updated correctly and that legitimate traffic is flowing as expected.

1.  **Review Network ACL Inbound Rules in Console**:
    *   Navigate back to the Amazon VPC console and select the Network ACL you modified.
    *   Go to the **Inbound Rules** tab and visually inspect the rules to ensure that:
        *   The unrestricted inbound rules (`0.0.0.0/0` to sensitive ports) have been removed or modified.
        *   New, specific rules for required traffic (with appropriate port ranges and restricted source CIDR blocks) are present and correctly configured.
        *   The rule numbers are in the intended order of evaluation.

2.  **Test Connectivity**:
    *   From a client machine within your allowed source IP range, attempt to connect to an instance in the associated subnet on the allowed ports (e.g., SSH to port 22, RDP to port 3389).
    *   If you allowed public HTTP/HTTPS traffic, verify that your web application is accessible from the internet.
    *   Attempt to connect from an *unauthorized* IP address to ensure that access is denied as expected.

### Next Steps

Consider the following actions to further enhance your security posture after remediating the Network ACLs:

1.  **Regular Review of Network ACLs**: Periodically review all Network ACLs in your VPCs to ensure that rules remain appropriate for your current application requirements and security policies.
2.  **Utilize Security Groups**: Remember that Network ACLs operate at the subnet level, while Security Groups operate at the instance level. For more granular control over instance traffic, ensure that your Security Groups are also configured with the principle of least privilege.
3.  **Implement Centralized Logging and Monitoring**: Enable VPC Flow Logs to capture information about the IP traffic going to and from network interfaces in your VPC. This data can be published to Amazon CloudWatch Logs or Amazon S3 and used for monitoring, troubleshooting, and security analysis.
4.  **Automate Security Checks**: Integrate automated tools and services (like AWS Security Hub, AWS Config, or custom scripts) to continuously monitor for and alert on misconfigured Network ACLs or other security policy violations.
5.  **Principle of Least Privilege**: Continuously apply the principle of least privilege to all your AWS resources, including Network ACLs and Security Groups, by allowing only the absolutely necessary traffic.
