# AZURE / Blob Service / Blob Container Private Access

## Quick Info

| | |
|-|-|
| **Plugin Title** | Blob Container Private Access |
| **Cloud** | AZURE |
| **Category** | Blob Service |
| **Description** | Ensures that all blob containers do not have anonymous public access set |
| **More Info** | Blob containers set with public access enables anonymous users to read blobs within a publicly accessible container without authentication. All blob containers should have private access configured. |
| **AZURE Link** | https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction |
| **Recommended Action** | Ensure each blob container is configured to restrict anonymous access |

## Detailed Remediation Steps
1. Log into the Microsoft Azure Management Console.
2. Find the search bar at the top and search for Storage account. </br> <img src="/resources/azure/blobservice/blob-container-private-access/step2.png"/>
3. Select the "Storage account" by clicking on the "Name" link to access the configuration changes. </br> <img src="/resources/azure/blobservice/blob-container-private-access/step3.png"/>
4. In the left navigation panel click on "Containers" under "Data storage".</br> <img src="/resources/azure/blobservice/blob-container-private-access/step4.png"/>
5. In the Containers list, select the container for which the "Anonymous access level" column shows "Blob" or "Container", and click the "Change access level" button at the top. If the "Anonymous access level" is "Private", the container is safe, and no action is required.</br> <img src="/resources/azure/blobservice/blob-container-private-access/step5.png"/>
6. In the "Change access level" pop up click on the "Anonymous access level" dropdown and select "Private(no anonymous access)" and click "OK" to make the necessary changes.<img src="/resources/azure/blobservice/blob-container-private-access/step6.png"/>
7. Repeat steps number 5 and 6 to ensure that all containers have private access level.</br>
