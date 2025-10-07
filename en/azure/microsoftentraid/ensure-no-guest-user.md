[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)

# AZURE / Microsoft Entra ID / Ensure No Guest User

## Quick Info

| | |
|-|-|
| **Plugin Title** | Ensure No Guest User |
| **Cloud** | AZURE |
| **Category** | Microsoft Entra ID |
| **Description** | Ensures that there are no guest users in the subscription |
| **More Info** | Guest users are usually users that are invited from outside the company structure, these users are not part of the onboarding/offboarding process and could be overlooked, causing security vulnerabilities. |
| **AZURE Link** | https://learn.microsoft.com/en-us/entra/external-id/add-users-administrator |
| **Recommended Action** | Remove all guest users unless they are required to be members of the Microsoft Entra ID account. |

## Detailed Remediation Steps
1. Log in to the Microsoft Azure Management Console.
2. Locate the search bar at the top of the portal, enter “Microsoft Entra ID” into the search field, and select the “Microsoft Entra ID” option from the search results.
3. Select "Users" under "Manage" on the left navigation panel.
4. In the users list, look for users with "User type" as "Guest". If there are "Guest" type users, then those users are not part of the onboarding/offboarding process and is considered a security vulnerability. Such accounts must be deleted.
5. Select all Users with "User type" as "Guest" and click "Delete" button located at the top of the page.
6. Click OK in the confirmation popup.
7. Repeat step number 3 to 6 for all other directories.