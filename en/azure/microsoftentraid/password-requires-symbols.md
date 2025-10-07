[![CloudSploit](https://cloudsploit.com/img/logo-new-big-text-100.png "CloudSploit")](https://cloudsploit.com)

# AZURE / Microsoft Entra ID / Password Requires Symbols

## Quick Info

| | |
|-|-|
| **Plugin Title** | Password Requires Symbols |
| **Cloud** | AZURE |
| **Category** | Microsoft Entra ID |
| **Description** | Ensures that all Azure passwords require symbol characters |
| **More Info** | Microsoft Entra ID handles most password policy settings, including which character types are required. It enforces at least three out of four of the following character types: lowercase, uppercase, special characters, and numbers. |
| **AZURE Link** | https://learn.microsoft.com/en-us/entra/identity/authentication/concept-sspr-policy#password-policies-that-only-apply-to-cloud-user-accounts |
| **Recommended Action** | No action necessary. Microsoft Entra ID handles password requirement settings. |

## Detailed Remediation Steps

1. Log in to the Microsoft Azure Management Console.
2. Locate the search bar at the top of the portal, enter “Microsoft Entra ID” into the search field, and select the “Microsoft Entra ID” option from the search results.
3. Select "Users" under "Manage" on the left navigation panel.
4. On the "Users" tab click on the "New User" option at the top.
5. On the "New User" dropdown, select "Create new user".
6. Under "Identity", enter details like "Username","Name", "First Name","Last Name".
7. Under "Password", click on "Let me create the password".
8. On the "Initial Tab" enter the password, and if the password doesn't contain any "Symbols such as @, #", Microsoft Entra ID will automatically display an error message when you click the "Create" button. 
9. If you click on the information bubble next to "Initial password" you will find the password combinations you can make.
10. Repeat the above steps to create New User with pre-defined "Password Requires Symbols".