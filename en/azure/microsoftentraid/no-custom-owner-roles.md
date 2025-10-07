# AZURE / Microsoft Entra ID / No Custom Owner Roles

## Quick Info

| | |
|-|-|
| **Plugin Title** | No Custom Owner Roles |
| **Cloud** | AZURE |
| **Category** | Microsoft Entra ID |
| **Description** | Ensures that no custom owner roles exist. |
| **More Info** | Subscription owners should not include permissions to create custom owner roles. This follows the principle of least privilege. |
| **AZURE Link** | https://docs.microsoft.com/en-us/azure/role-based-access-control/custom-roles |
| **Recommended Action** | Remove roles that allow permissions to create custom owner roles. |

## Detailed Remediation Steps

1. Log in to the Microsoft Azure Management Console.
2. Locate the search bar at the top of the portal, enter “Microsoft Entra ID” into the search field, and select the “Microsoft Entra ID” option from the search results.
3. Select "Roles and Administrators" under "Manage" on the left navigation panel.
4. Look for custom roles with "Owner" permissions.
5. Click on "Roles" to view all roles.
6. Use the search bar to filter roles by keyword "Owner".
7. Select any custom owner roles found. 
8. Review the role's permissions.
9. Ensure that the role does not contain elevated privileges such as "Microsoft.Authorization/roleAssignments/write" or "Microsoft.Authorization/roleDefinitions/write". 
10. Remove the custom owner roles if they exist. Click on the role.
11. Select "Delete" to remove the role.