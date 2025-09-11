[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)

# AZURE / CDN Profiles / Endpoint Logging Enabled

## Quick Info

| | |
|-|-|
| **Plugin Title** | Endpoint Logging Enabled |
| **Cloud** | AZURE |
| **Category** | CDN Profiles |
| **Description** | Ensures that endpoint requests are being logged for CDN endpoints |
| **More Info** | Endpoint Logging ensures that all requests to a CDN endpoint are logged. |
| **AZURE Link** | https://docs.microsoft.com/en-us/azure/cdn/cdn-azure-diagnostic-logs |
| **Recommended Action** | Ensure that diagnostic logging is enabled for each CDN endpoint for each CDN profile. |

## Detailed Remediation Steps
1. Log into the Microsoft Azure portal.
2. Select the "Search resources, services, and docs" option at the top and search for "All resources" and select it from the results. Alternatively, "All resources" is available directly under the Azure services section on the homepage.</br> <img src="/resources/azure/cdnprofiles/endpoint-logging-enabled/step2.png"/>
3. Use the resource type filter to choose "Front Door", then click Apply.</br> <img src="/resources/azure/cdnprofiles/endpoint-logging-enabled/step3.png"/>
4. Click on the "Name" link of the CDN profile to access the configuration changes.</br> <img src="/resources/azure/cdnprofiles/endpoint-logging-enabled/step4.png"/>
5. In the left navigation panel, click on the "Diagnostic settings" under "Monitoring".</br> <img src="/resources/azure/cdnprofiles/endpoint-logging-enabled/step5.png"/>
6. In the "Diagnostic setting" panel if you see "No diagnostic settings defined" then logging is not enabled for this CDN. This is against the Azure best practices. Now click on the "+ Add diagnostic setting" link to enable diagnostic logging.</br> <img src="/resources/azure/cdnprofiles/endpoint-logging-enabled/step6.png"/>
7. In the "Diagnostic setting" page that opens, enter Diagnostic setting name.</br> <img src="/resources/azure/cdnprofiles/endpoint-logging-enabled/step7.png"/>
8. Select "allLogs" under "Logs".
9. Under "Metrics" select "AllMetrics".
10. Under "Destination details" select "Send to Log Analytics workspace".
11. Click "Save" at the top of the page to save the changes and enable logging.</br> <img src="/resources/azure/cdnprofiles/endpoint-logging-enabled/step11.png"/>
12. Repeat steps 4 - 11 for all other endpoints in each CDN profile.
