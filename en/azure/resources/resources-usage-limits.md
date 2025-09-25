[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)

# AZURE / Resources / Resources Usage Limits

## Quick Info

| | |
|-|-|
| **Plugin Title** | Resources Usage Limits |
| **Cloud** | AZURE |
| **Category** | Resources |
| **Description** | Determines if resources are close to the Azure per-account limit |
| **More Info** | Azure limits accounts to certain numbers of resources. Exceeding those limits could prevent resources from launching. |
| **AZURE Link** | https://docs.microsoft.com/en-us/azure/azure-subscription-service-limits |
| **Recommended Action** | Check if resources are close to the account limit to avoid resource launch failures |

## Detailed Remediation Steps

1. Log in to the Microsoft Azure Management Console.

2. Select for "subscriptions" in the search bar on top and select the same. </br> <img src="/resources/azure/resources/resources-usage-limits/step2.png"/>

3. Select your subscription by clicking on the subscription name. </br> <img src="/resources/azure/resources/resources-usage-limits/step3.png"/>

4. In the left navigation panel, click on "Usage + quotas" under "Settings". </br> <img src="/resources/azure/resources/resources-usage-limits/step4.png"/>

5. Use the "Region" dropdown to select each region where you have resources deployed. In this case, "all" regions are selected. </br> <img src="/resources/azure/resources/resources-usage-limits/step5.png"/>

6. Review the usage table and identify resources with high usage percentages as recommended below:
   - **70-89% usage**: Warning level - monitor closely
   - **90-99% usage**: High risk - take immediate action  
   - **100%+ usage**: Critical - emergency action required
   In this case, all the resources are within the acceptable usage limit. </br> <img src="/resources/azure/resources/resources-usage-limits/step6.png"/>

7. For resources at 90%+ usage, click on "Resources" in the main Azure menu. </br> <img src="/resources/azure/resources/resources-usage-limits/step7.png"/>

8. Filter by the resource type that's approaching limits (e.g., "Virtual machines", "Storage accounts"). </br> <img src="/resources/azure/resources/resources-usage-limits/step8.png"/>

9. Identify and delete unused resources. Some common examples are:
   - Look for resources with "Stopped (deallocated)" status
   - Find disks with "Unattached" state
   - Locate network interfaces without associated VMs
   - Find public IPs not associated with any resources

10. For resources at 100% usage, return to "Usage + quotas", and click on 'edit' icon which is set as 'yes' under 'Adjustable'. </br> <img src="/resources/azure/resources/resources-usage-limits/step10.png"/>

11. Click the 'Request adjustment button to submit a quota increase request. </br> <img src="/resources/azure/resources/resources-usage-limits/step11.png"/>

12. For a particular resource, if the 'Adjustable' is 'no', then a support request form can be filled with business justificationcan. </br> <img src="/resources/azure/resources/resources-usage-limits/step12.png"/>

13. If needed, an alert can be created for a particular resource type. </br> <img src="/resources/azure/resources/resources-usage-limits/step13.png"/>

14. Repeat steps 5-13 for the other resources accordingly.
