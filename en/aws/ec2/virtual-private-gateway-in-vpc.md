# AWS / EC2 / Virtual Private Gateway In VPC

## Quick Info

| | |
|-|-|
| **Plugin Title** | Virtual Private Gateway In VPC |
| **Cloud** | AWS |
| **Category** | EC2 |
| **Description** | Ensure Virtual Private Gateways are associated with at least one VPC. |
| **More Info** | Virtual Private Gateways allow communication between cloud infrastructure and the remote customer network. They help in establishing VPN connection between VPC and the customer gateway. Make sure virtual private gateways are always associated with a VPC to meet security and regulatory compliance requirements within your organization. |
| **AWS Link** | https://docs.aws.amazon.com/vpn/latest/s2svpn/SetUpVPNConnections.html |
| **Recommended Action** | Check if Virtual Private Gateways have VPC associated. |

### Introduction
A Virtual Private Gateway (VPG) is a crucial component in AWS that enables communication between your Amazon Virtual Private Cloud (VPC) and your on-premises networks via a Site-to-Site VPN connection. It acts as the VPN concentrator on the Amazon side of the Site-to-Site VPN connection. Ensuring that your Virtual Private Gateways are associated with at least one VPC is vital for establishing secure and compliant network connectivity, allowing your cloud infrastructure to communicate with remote customer networks. An unassociated VPG cannot fulfill its purpose of facilitating VPN connections, leaving a potential gap in your network architecture and compliance posture.

### Prerequisites
Before proceeding with the remediation steps, ensure the following:

*   **Target VPC**: You have an existing Amazon VPC in the same AWS Region where the unassociated Virtual Private Gateway resides, to which you intend to attach the VPG.

### Detailed Remediation Steps

Follow these steps to associate an unassociated Virtual Private Gateway with a VPC using the AWS Management Console:

1.  **Open the AWS Direct Connect Console**: Navigate to the AWS Direct Connect console at [https://console.aws.amazon.com/directconnect/v2/home](https://console.aws.amazon.com/directconnect/v2/home). Although VPGs are primarily an EC2 service component, the Direct Connect console provides a convenient interface for managing them, especially when considering hybrid connectivity.
2.  **Navigate to Virtual Private Gateways**: In the left-hand navigation pane, under **AWS Direct Connect**, choose **Virtual Private Gateways**.
3.  **Identify and Select the Unassociated VPG**: From the list of Virtual Private Gateways, locate the one identified as unassociated. You can typically identify its state in the "State" column or by checking its details for VPC attachments. Select the checkbox next to the target Virtual Private Gateway.
4.  **Initiate Attachment Action**: With the VPG selected, choose the **Actions** dropdown menu, and then select **Attach to VPC**.
5.  **Select Target VPC and Confirm**:
    *   In the **Attach to VPC** dialog box, select the desired VPC from the **VPC** dropdown list. Ensure this VPC is in the same AWS Region as your Virtual Private Gateway.
    *   Review your selection and then choose **Yes, Attach** to confirm the association.

The Virtual Private Gateway will now begin the process of attaching to the selected VPC. The state of the VPG will change to `attaching` and then to `attached` once the operation is complete.

### Verification

To verify that the Virtual Private Gateway has been successfully associated with the VPC:

1.  **Check VPG State in Console**:
    *   Return to the **Virtual Private Gateways** section in the AWS Direct Connect console.
    *   Locate the Virtual Private Gateway you just modified.
    *   Verify that its **State** is now `attached` and that the **VPC** column displays the ID of the VPC you associated it with.
        
