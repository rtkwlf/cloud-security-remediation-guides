# AZURE / Defender / Application Whitelisting Enabled

## Quick Info

| | |
|-|-|
| **Plugin Title** | Application Whitelisting Enabled |
| **Cloud** | AZURE |
| **Category** | Defender |
| **Description** | Ensures that Security Center Monitor Adaptive Application Whitelisting is enabled |
| **More Info** | Adaptive application controls work in conjunction with machine learning to analyze processes running in a VM and help control which applications can run, hardening the VM against malware. |
| **AZURE Link** | https://docs.microsoft.com/en-us/azure/security-center/security-center-adaptiveapplication |
| **Recommended Action** | Enable Adaptive Application Controls for Virtual Machines from the Azure Security Center by ensuring AuditIfNotExists setting is used. |

## Detailed Remediation Steps

1. Log in to the Microsoft Azure Management Console.
2. Select the "Search resources, services, and docs" option at the top and search for "Policy" and select the "Policy". </br> <img src="/resources/azure/defender/application-whitelisting-enabled/step2.png"/>
3. Scroll down the left navigation panel and select "Compliance".</br> <img src="/resources/azure/defender/application-whitelisting-enabled/step3.png"/>
4. On the "Policy | Compliance" page, under "Name" column select compliance for the "Scope" of necessary Subscription.</br> <img src="/resources/azure/defender/application-whitelisting-enabled/step4.png"/>
5. On the "Policy | Compliance" page select the "View Assignment" Tab on the top.</br> <img src="/resources/azure/defender/application-whitelisting-enabled/step5.png"/>
6. On the "Policy | Compliance | Subscription" page, Select the "Edit Assignment" Tab at the top.</br> <img src="/resources/azure/defender/application-whitelisting-enabled/step6.png"/>
7. On the Assign Initiative page, select the "Parameters" tab and uncheck "Only show parameters that need input or review". It will show you a list of parameters.</br> <img src="/resources/azure/defender/application-whitelisting-enabled/step7.png"/>
8. In the list search for the setting "Adaptive Application Controls for defining safe applications should be enabled on your machines". If it's set to "Disabled" then "Adaptive Application Whitelisting" is not enabled on the selected "Subscription".</br> <img src="/resources/azure/defender/application-whitelisting-enabled/step8.png"/>
9. To enable "Adaptive Application Whitelisting" click to open the dropdown of "Adaptive Application Controls should be enabled on virtual machines" and select the "AuditIfNotExists" option. </br> <img src="/resources/azure/defender/application-whitelisting-enabled/step9.png"/>
10. Click on the "Review + save" button to make the necessary changes.</br> <img src="/resources/azure/defender/application-whitelisting-enabled/step10.png"/>
11. Repeat step number 3 - 10 to ensures "Adaptive Application Whitelisting" is enabled for Subscriptions.</br>
