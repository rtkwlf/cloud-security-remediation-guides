# AZURE / Defender / Security Contacts Enabled

## Quick Info

| | |
|-|----------------------------------------------------------------------------------------------------------------------------------------------|
| **Plugin Title** | Security Contacts Enabled |
| **Cloud** | AZURE |
| **Category** | Defender |
| **Description** | Ensures that security contact phone number and email address are set |
| **More Info** | Setting security contacts ensures that any defender incidents detected by Azure are sent to a defender team equipped to handle the incident. |
| **AZURE Link** | https://learn.microsoft.com/en-us/azure/defender-for-cloud/configure-email-notifications |
| **Recommended Action** | Ensure that email notifications and phone numbers are configured for the subscription from the Microsoft Defender for Cloud. |

## Detailed Remediation Steps

### Method 1: Azure CLI (Recommended for Phone Numbers)

**Note**: Azure Portal UI often doesn't expose phone number configuration. Azure CLI provides the most reliable method.

```bash
# Verify current configuration
az security contact list --output table

# Create/update security contact with complete configuration
az security contact create \
    --name 'default' \
    --emails 'your-existing-email@yourcompany.com' \
    --phone '+1-555-123-4567' \
    --notifications-by-role '{"state":"On","roles":["Owner"]}' \
    --alert-notifications '{"state":"On","minimalSeverity":"Low"}'

# Verify the update
az security contact list --output table
```

### Method 2: Azure PowerShell

```powershell
# Connect to Azure
Connect-AzAccount

# Create/update security contact
Set-AzSecurityContact -Name "default" `
    -Email "your-existing-email@yourcompany.com" `
    -Phone "+1-555-123-4567" `
    -AlertNotifications On `
    -AlertsToAdmins On `
    -MinimalSeverity Low

# Verify the configuration
Get-AzSecurityContact
```

### Method 3: Azure Portal (Limited - Email Only)

**⚠️ Important**: The Azure Portal typically only allows email configuration. Phone numbers usually require CLI/PowerShell.
## Detailed Remediation Steps (Azure Portal)
1. Log in to the Microsoft Azure Management Console.
2. Select the "Search resources, services, and docs" option at the top and search for "Microsoft Defender for Cloud". </br> <img src="/resources/azure/defender/security-contacts-enabled/step2.png"/>
3. On the "Microsoft Defender for Cloud" page, scroll down the left navigation panel and choose "Environment Settings" under "Management". </br> <img src="/resources/azure/defender/security-contacts-enabled/step3.png"/>
4. On the "Environment Settings" page, select the "Subscription" by clicking on the "Name". </br> <img src="/resources/azure/defender/security-contacts-enabled/step4.png"/>
5. Under the "Settings | Defender plans " page, click on the "Email Notifications". </br> <img src="/resources/azure/defender/security-contacts-enabled/step5.png"/>
6. On the "Settings | Email notifications" page under "Email recipients" if the "Additional email addresses (separated by commas)" is empty and only "owner" is selected in "All users with the following roles" then the defender alerts are not configured to be sent to the admins. </br> <img src="/resources/azure/defender/security-contacts-enabled/step6.png"/>
7. Under "Email recipients", click the dropdown for "All users with the following roles" and check mark "AccountAdmin" and "ServiceAdmin" along with owner and enter one or more than one "Email addresses" separated by comma in section "Additional email addresses (separated by commas)". </br> <img src="/resources/azure/defender/security-contacts-enabled/step7.png"/>
8. Under "Notification types" select "High" from the dropdown next to "Notify about alerts with the following severity (or higher)". Click on the "Save" button to make the changes. </br> <img src="/resources/azure/defender/security-contacts-enabled/step8.png"/>
9. Repeat steps 3-8 to ensure that email notifications are configured to be sent. </br>

**Note**: To configure phone numbers for complete security contact setup, use Method 1 (Azure CLI) or Method 2 (PowerShell) described above

### Notification Severity Levels
Choose the appropriate severity level based on your organization's needs:
"minimalSeverity":"Low" = Receive ALL alerts (Low + Medium + High severity)
"minimalSeverity":"Medium" = Receive Medium and High severity alerts only
"minimalSeverity":"High" = Receive High severity alerts only
Microsoft limits email frequency to prevent alert fatigue. Higher severity alerts have higher daily limits.
https://learn.microsoft.com/en-us/azure/defender-for-cloud/configure-email-notifications