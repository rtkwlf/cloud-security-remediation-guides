[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)

# AZURE / CDN Profiles / Detect Insecure Custom Origin

## Quick Info

| | |
|-|-|
| **Plugin Title** | Detect Insecure Custom Origin |
| **Cloud** | AZURE |
| **Category** | CDN Profiles |
| **Description** | Ensures that HTTPS is enabled for CDN endpoints with a custom origin |
| **More Info** | All Azure CDN endpoints should enable HTTPS to secure traffic to the backend custom origin. |
| **AZURE Link** | https://docs.microsoft.com/en-us/azure/cdn/cdn-create-endpoint-how-to |
| **Recommended Action** | Enable HTTPS and disable HTTP for each custom origin endpoint for each CDN profile. |

## Detailed Remediation Steps
1. Log into the Microsoft Azure portal.
2. Select the "Search resources, services, and docs" option at the top and search for "All resources" and select it from the results. Alternatively, "All resources" is available directly under the Azure services section on the homepage.</br> <img src="/resources/azure/cdnprofiles/detect-insecure-custom-origin/step2.png"/>
3. Use the resource type filter to choose "Front Door", then click Apply.</br> <img src="/resources/azure/cdnprofiles/detect-insecure-custom-origin/step3.png"/>
4. Click on the "Name" link of the CDN profile to access the configuration changes.</br> <img src="/resources/azure/cdnprofiles/detect-insecure-custom-origin/step4.png"/>
5. In the CDN details pane that opens, click on the "Endpoints" link under "Properties".</br> <img src="/resources/azure/cdnprofiles/detect-insecure-custom-origin/step5.png"/>
6. On the endpoint management page that opens, click on "default-route" under "Routes" column to load the route configuration page.</br> <img src="/resources/azure/cdnprofiles/detect-insecure-custom-origin/step6.png"/>
7. On the "Update route" page, check the value of "Accepted protocols" dropdown. If it is set to "HTTP only" or "HTTP and HTTPS" then the endpoint allows insecure traffic. This is a security threat.</br> <img src="/resources/azure/cdnprofiles/detect-insecure-custom-origin/step7.png"/>
8. Click on the "Accepted protocols" dropdown and select "HTTPS only". This will configure the endpoint to accept only secure traffic.
9. Ensure that the checkbox for "Redirect" is selected to "Redirect all traffic to use HTTPS".</br> <img src="/resources/azure/cdnprofiles/detect-insecure-custom-origin/step9.png"/>
10. Click "Update" at the bottom of the page to save the changes.
11. Repeat steps 4 - 10 for all other endpoints in each CDN profile.
