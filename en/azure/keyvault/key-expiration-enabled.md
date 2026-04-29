# AZURE / Key Vault / Key Expiration Enabled

## Quick Info

| | |
|-|-|
| **Plugin Title** | Key Expiration Enabled |
| **Cloud** | AZURE |
| **Category** | Key Vault |
| **Description** | Ensure that all Keys in Azure Key Vault have an expiry time set. |
| **More Info** | Setting an expiry time on all keys forces key rotation and removes unused and forgotten keys from being used. |
| **AZURE Link** | https://docs.microsoft.com/en-us/azure/key-vault/about-keys-secrets-and-certificates |
| **Recommended Action** | 1. Go to Key vaults. 2. For each Key vault, click on Keys. 3. Ensure that each key in the vault has EXPIRATION DATE set as appropriate. |

## Detailed Remediation Steps
1. Log into the Microsoft Azure Management Console.
2. Select the "Search resources, services, and docs" option at the top and search for Key Vault . </br> <img src="/resources/azure/keyvault/key-expiration-enabled/step2.png"/>
3. On the "Key vault" page, select the "Key Vault" for which keys need to be verified.</br> <img src="/resources/azure/keyvault/key-expiration-enabled/step3.png"/>
4. Scroll down and click "Keys" from the navigation pane on the left. Then, from the list of keys, select key with no expiration date under "Expiration date" column.</br> <img src="/resources/azure/keyvault/key-expiration-enabled/step4.png"/>
5. In the key versions pane that opens, click "Rotation Policy" button at the top.</br> <img src="/resources/azure/keyvault/key-expiration-enabled/step5.png"/>
6. In the Rotation policy pane, click on the Expiry time textbox and enter 28. From the units dropdown next to the textbox, select "days".</br> <img src="/resources/azure/keyvault/key-expiration-enabled/step6.png"/>
7. Under the Rotation section, "Enable auto rotation" by selecting the "Enabled" radio button.</br> <img src="/resources/azure/keyvault/key-expiration-enabled/step7.png"/>
8. Select "Automatically renew at a given time after creation" for "Rotation option".
9. For "Rotation time" enter 18 and select "days" as the unit of time.
10. Finally, hit "Save" at the top of the pane to complete the changes.</br> <img src="/resources/azure/keyvault/key-expiration-enabled/step8.png"/>
11. Repeat step number 3 - 10 for all other key vaults and keys without expiration date.
