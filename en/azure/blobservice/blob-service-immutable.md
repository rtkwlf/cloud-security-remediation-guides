# AZURE / Blob Service / Blob Service Immutable

## Quick Info

| | |
|-|-|
| **Plugin Title** | Blob Service Immutable |
| **Cloud** | AZURE |
| **Category** | Blob Service |
| **Description** | Ensures data immutability is properly configured for blob services to protect critical data against deletion |
| **More Info** | Immutable storage helps store data securely by protecting critical data against deletion. |
| **AZURE Link** | https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-immutable-storage#Getting-started |
| **Recommended Action** | Enable a data immutability policy for all storage containers in the Azure storage account. |

## Detailed Remediation Steps
1. **Log in** to the [Microsoft Azure Management Console](https://portal.azure.com/).
2. Find the search bar at the top and search for "Storage accounts". </br> <img src="/resources/azure/blobservice/blob-service-immutable/step2.png"/>
3. Select the desired Storage Account by clicking on the **Name** link. </br> <img src="/resources/azure/blobservice/blob-service-immutable/step3.png"/>
4. In the left navigation panel, click on **"Containers"** under **"Data Storage"**. </br> <img src="/resources/azure/blobservice/blob-service-immutable/step4.png"/>
5. In the Containers list, locate the target container. Click the **three dots (...)** on the far right and select **"Access policy"**. </br> <img src="/resources/azure/blobservice/blob-service-immutable/step5.png"/>
6. In the **"Access Policy"** panel, configure an **immutable blob storage policy**:  
   - Click **"Add policy"**  
   - Choose **"Time-based retention"** or **"Legal hold"**:  
     - **Time-based retention**: Set a fixed retention period in days (e.g., enter `2555` days for 7-year compliance).  
     - **Legal hold**: Use this to lock data indefinitely until the hold is explicitly removed (no retention period required).  
   - Optionally specify the **policy scope** (container or version-level):  
     - To apply at the container level, **do not check** the box for *"Enable version-level immutability"*  
   - (Optional) Enable the checkbox for **"Allow protected append writes to"**:  
     - Determines whether additional append actions will be allowed on blobs in this container.  
     - ⚠️ **Note:** This setting cannot be modified while a policy is locked.  
   - Click **"Save"** to apply the policy. </br> <img src="/resources/azure/blobservice/blob-service-immutable/step6.png"/> </br>
   **Examples**:  
   - For regulatory compliance (e.g., financial records), use **Time-based retention: 2555 days (7 years)**  
   - For legal investigations, use a **Legal hold** to prevent deletion until the case is resolved  
   - For audit logging, enable **"Allow protected append writes to"** to allow appending while maintaining immutability
7. To enforce data immutability across multiple containers, **repeat Steps 5–6** for each container containing critical or sensitive data. <br/>