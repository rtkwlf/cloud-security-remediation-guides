[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)

# AZURE / App Service / HTTPS Only Enabled

## Quick Info

| | |
|-|-|
| **Plugin Title** | HTTPS Only Enabled |
| **Cloud** | AZURE |
| **Category** | App Service |
| **Description** | Ensures HTTPS Only is enabled for App Services, redirecting all HTTP traffic to HTTPS |
| **More Info** | Enabling HTTPS Only traffic will redirect all non-secure HTTP requests to HTTPS. HTTPS uses the SSL/TLS protocol to provide a secure connection. |
| **AZURE Link** | https://docs.microsoft.com/en-us/azure/app-service/app-service-web-tutorial-custom-ssl#enforce-https |
| **Recommended Action** | Enable HTTPS Only support SSL settings for all App Services |

## Detailed Remediation Steps

1. Log into the Microsoft Azure Management Console.
2. Select the "Search resources, services, and docs" option at the top and search for App Services. </br> <img src="/resources/azure/appservice/https-only-enabled/step2.png"/>
3. Select the "App Services" by clicking on the "Name" link to access the configuration changes.</br> <img src="/resources/azure/appservice/https-only-enabled/step3.png"/>
4. Scroll down the selected "App Services" left navigation panel and in "Settings" click on the "Configuration" option.</br> <img src="/resources/azure/appservice/https-only-enabled/step4.png"/>
5. On the "Configuration" page select the General settings tab, scroll down to "HTTPs Only". If the "App Service" is not using "HTTPS only" then select "On". </br> <img src="/resources/azure/appservice/https-only-enabled/step5.png"/>
6. Click Save.</br> <img src="/resources/azure/appservice/https-only-enabled/step6.png"/>
7. Wait for the confirmation box, then click "Continue".</br> <img src="/resources/azure/appservice/https-only-enabled/step7.png"/>