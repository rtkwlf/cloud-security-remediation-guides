# AZURE / Microsoft Entra ID / Minimum Password Length

## Quick Info

| | |
|-|-|
| **Plugin Title** | Minimum Password Length |
| **Cloud** | AZURE |
| **Category** | Microsoft Entra ID |
| **Description** | Ensures that all Azure passwords require a minimum length |
| **More Info** | Microsoft Entra ID handles most password policy settings, including the minimum password length, defaulted to 8 characters. |
| **AZURE Link** | https://learn.microsoft.com/en-us/entra/identity/authentication/concept-sspr-policy#password-policies-that-only-apply-to-cloud-user-accounts |
| **Recommended Action** | No action necessary. Microsoft Entra ID handles password requirement settings. |

## Detailed Remediation Steps
1. Log in to the Microsoft Azure Management Console.
2. Locate the search bar at the top of the portal, enter “Microsoft Entra ID” into the search field, and select the “Microsoft Entra ID” option from the search results.
3. Select "Users" under "Manage" on the left navigation panel.
4. On the "Users" tab click on the "New User" tab at the top.
5. On the "New User" dropdown, select the option "Create new user".
6. Under the "Identity", enter details like "Username","Name", "First Name","Last Name".
7. Under the "Password", select "Let me create the password".
8. In the "Initial password" textbox, enter the password. If it's less than eight characters, Microsoft Entra ID will show this error: "The value must have a length of at least 8".
9. Repeat the above steps to create New User with pre-defined "Minimum Password Length".