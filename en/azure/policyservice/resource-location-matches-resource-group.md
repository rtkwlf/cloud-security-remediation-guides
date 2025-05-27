[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)

# AZURE / Azure Policy / Resource Location Matches Resource Group

## Quick Info

| | |
|-|-|
| **Plugin Title** | Resource Location Matches Resource Group |
| **Cloud** | AZURE |
| **Category** | Azure Policy |
| **Description** | Ensures a policy is configured to audit that deployed resource locations match their resource group locations |
| **More Info** | Using Azure Policy to monitor resource location compliance helps ensure that new resources are not launched into locations that do not match their resource group. |
| **AZURE Link** | https://docs.microsoft.com/en-us/azure/governance/policy/assign-policy-portal |
| **Recommended Action** | Enable the built-in Azure Policy definition: Audit resource location matches resource group location. |

## Detailed Remediation Steps
1. Sign in to the [Azure portal](portal.azure.com).
2. Search for policy and select it from the list. </br> <img src="/resources/azure/policyservice/resource-location-matches-resource-group/step2.png"/>
3. On the "Policy" page, scroll down the left navigation panel and choose "Assignments" under "Authoring".</br> <img src="/resources/azure/policyservice/resource-location-matches-resource-group/step3.png"/>
4. On the "Policy Assignments" page, check the "Policies" listed and if there are no "Policies" for "Resource Location Matches Resource Group" then the selected "Assignment" don't have any "Resource Location Matches Resource Group" policy.</br> <img src="/resources/azure/policyservice/resource-location-matches-resource-group/step4.png"/>
5. If there is no policy for "Resource Location Matches Resource Group" then click on "Assign policy" at the top to create a new policy.</br> <img src="/resources/azure/policyservice/resource-location-matches-resource-group/step5.png"/>
6. On the "Assign Policy" page, under "Basics" tab, select "Scope" accordingly and click on the "..." dots icon next to "Policy definition".</br> <img src="/resources/azure/policyservice/resource-location-matches-resource-group/step6.png"/>
7. On the "Available Definitions" page, click on the "Search" box and search for "Resource Location Matches Resource Group". Click the Policy Definition found and then click "Add" button at the bottom.</br> <img src="/resources/azure/policyservice/resource-location-matches-resource-group/step7.png"/>
8. Once back on the "Assign Policy" page, provide a "Description" and click on the "Next" to view Parameters tab. If the policy definition you selected on the Basics tab has parameters, you configure them on the Parameters tab. Click Next to view Remediation tab.
9. On the "Remediation" tab, click on the checkbox next to the "Create a Managed Identity" and select desired "Managed Identity Location". Click "Review + create" button at the bottom.</br> <img src="/resources/azure/policyservice/resource-location-matches-resource-group/step9.png"/>
10. On the "Review + Create" tab, click "Create" button at the bottom to create the specific "Resource Location Matches Resource Group" policy.</br> <img src="/resources/azure/policyservice/resource-location-matches-resource-group/step10.png"/>
11. Repeat steps number 6 - 10 to enable the built-in "Azure Policy definition: Audit resource location matches resource group location" for all directories.</br>
