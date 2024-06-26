[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)

# GOOGLE / Resource Manager / Disable Service Account Key Upload

## Quick Info

| | |
|-|-|
| **Plugin Title** | Disable Service Account Key Upload |
| **Cloud** | GOOGLE |
| **Category** | Resource Manager |
| **Description** | Determine if "Disable Service Account Key Upload" policy is enforced at the GCP organization level. |
| **More Info** | User-managed keys can impose a security risk if they are not handled correctly. To minimize the risk, enable user-managed keys in only specific locations. |
| **GOOGLE Link** | https://cloud.google.com/resource-manager/docs/organization-policy/org-policy-constraints |
| **Recommended Action** |Ensure that "Disable Service Account Key Upload" constraint is enforced at the organization level. |

## Detailed Remediation Steps
1. Sign in to Google Cloud Management Console with the organizational unit credentials, then Click the deployment selector in the upper navigation bar.</br></br> <img src="/resources/google/resourcemanager/disable-service-account-key-upload/step1.png"/></br>
2. Select ALL to view a summary of all current deployments, and then pick the Google Cloud organisation you want to look at.</br></br> <img src="/resources/google/resourcemanager/disable-service-account-key-upload/step2.png"/></br>
3. Navigate to Cloud [Identity and Access Management IAM](https://console.cloud.google.com/iam-admin/iam).
4. In the left navigation panel, select Organization Policies to view the list of the constraint policies available for your GCP organization.</br> <img src="/resources/google/resourcemanager/disable-service-account-key-upload/step4.png"/></br></br>
5. Click inside Filter box, filter by **Name**. </br>
6. Type in **Disable Service Account Key Upload** to return the \"Disable Service Account Key Upload\" policy.</br><img src="/resources/google/resourcemanager/disable-service-account-key-upload/step6.png"/></br></br>
7. Click on the policy name. </br> <img src="/resources/google/resourcemanager/disable-service-account-key-upload/step7.png"/></br></br>
8. On the Policy details page, check the **Status** attribute value. If the **Status** attribute value is set to **Not enforced**, then click on \"Manage Policy\" to edit the policy.</br> <img src="/resources/google/resourcemanager/disable-service-account-key-upload/step8.png"/></br></br>
9. In Edit Policy screen, under \"Applies to\" section, select \"Customize\". Then Click on \"Add Role\".</br> <img src="/resources/google/resourcemanager/disable-service-account-key-upload/step9.png"/></br></br>
10. In Add Role expanded Section, under enforcemnt: Select "On".</br></br> <img src="/resources/google/resourcemanager/disable-service-account-key-upload/step10.png"/></br>
11. Click Save. </br> <img src="/resources/google/resourcemanager/disable-service-account-key-upload/step11.png"/></br>
12. When return to Policy details screen, status will now show "Enforced". </br> <img src="/resources/google/resourcemanager/disable-service-account-key-upload/step12.png"/></br>