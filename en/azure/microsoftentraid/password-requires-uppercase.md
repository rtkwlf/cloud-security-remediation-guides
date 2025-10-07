# AZURE / Microsoft Entra ID / Password Requires Uppercase

## Quick Info

| | |
|-|-|
| **Plugin Title** | Password Requires Uppercase |
| **Cloud** | AZURE |
| **Category** | Microsoft Entra ID |
| **Description** | Ensures that all Azure passwords require uppercase characters |
| **More Info** | Microsoft Entra ID handles most password policy settings, including which character types are required. It enforces at least three out of four of the following character types: lowercase, uppercase, special characters, and numbers. |
| **AZURE Link** | https://learn.microsoft.com/en-us/entra/identity/authentication/concept-sspr-policy#password-policies-that-only-apply-to-cloud-user-accounts |
| **Recommended Action** | No action necessary. Microsoft Entra ID handles password requirement settings. |

## Detailed Remediation Steps

1. Log in to the Microsoft Azure Management Console.
2. Locate the search bar at the top of the portal, enter “Microsoft Entra ID” into the search field, and select the “Microsoft Entra ID” option from the search results.
3. Select "Users" under "Manage" on the left navigation panel.
4. On the "Users" tab click on the "New User" option at the top.
5. On the "New User" dropdown, click on the "Create new user".
6. Under the "Identity", enter details like "Username","Name", "First Name","Last Name". Select the group if required and define the role for the user.
7. On the "Password" tab, click on the "Let me create the password". 
8. If the password does not contain a uppercase letter, Microsoft Entra ID will automatically display an error message when you click the "Create" button.
9. Repeat the above steps to create New User with pre-defined "Password Requires Uppercase".