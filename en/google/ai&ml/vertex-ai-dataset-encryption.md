# GOOGLE / AI & ML / Vertex AI Dataset Encryption

## Quick Info

| | |
|-|-|
| **Plugin Title** | Vertex AI Dataset Encryption |
| **Cloud** | GOOGLE |
| **Category** | AI & ML |
| **Description** | Ensure that Vertex AI datasets are encrypted using desired encryption protection level. |
| **More Info** | By default Google encrypts all datasets using Google-managed encryption keys. To have more control over the encryption process of your Vertex AI datasets you can use Customer-Managed Keys (CMKs). |
| **GOOGLE Link** | https://cloud.google.com/vertex-ai/docs/general/cmek |
| **Recommended Action** | Recreate existing datasets with desired protection level. |

---

## Introduction

This guide addresses the remediation of Vertex AI datasets that do not meet the desired encryption protection level. The detection script identifies datasets with encryption levels below the configured threshold (default: cloudcmek) and requires recreation with appropriate CMEK integration to achieve compliance.

## Prerequisites

#### Obtain Required IAM Roles

- Cloud KMS Admin (`roles/cloudkms.admin`) IAM role granted on the project or parent resource, required to create key rings and Customer-Managed Encryption Keys (CMEKs) during remediation.
- Vertex AI Admin (`roles/aiplatform.admin`) IAM role granted on the project, required to create, manage, export, and delete Vertex AI datasets during remediation.

## Remediation Steps

> ⚠️ **Warning:** Remediating this finding requires deleting and recreating existing Vertex AI datasets. This will result in data loss if datasets are not backed up, and may disrupt active training workflows or model serving pipelines that depend on these datasets. Coordinate with stakeholders before proceeding.

#### Recreate Vertex AI Datasets with CMEK Encryption Specification

1. Sign in to the [Google Cloud Management Console](https://console.cloud.google.com/).
2. Select the GCP project you want to configure from the console top navigation bar.
3. Back up existing Vertex AI dataset data:
   - A. Navigate to the [Vertex AI Datasets console](https://console.cloud.google.com/vertex-ai/datasets).
   - B. Identify the dataset(s) that need to be recreated with CMEK encryption.
   - C. Export the dataset by clicking the 3 dots on the right of the dataset name, selecting **Export dataset** and then choose the right **Destination folder** in a Cloud Storage bucket. 
   - D. Choose **Export** to start the export operation. Ensure the exported data is securely stored and accessible for re-import after dataset recreation.
4. Create and configure a new Cloud KMS Customer-Managed Encryption Key (Skip this step if you already have a CMEK that you want to use for dataset recreation):
   - A. Navigate to [Key Management Service (KMS) console](https://console.cloud.google.com/security/kms).
   - B. Choose **+ Create Key Ring** from the top menu to set up the required key ring.
   - C. On the **Create key ring** page, provide a unique name in the **Key ring name** box, select the appropriate **Location type**, then choose a location for the key ring from the **Region/Multi-region** dropdown list. Ensure the key ring location matches the region of the Vertex AI dataset. Choose **Create** to deploy the new key ring.
   - D. On the **Create key** setup page, provide a name for your new key in the **Key name** box, choose the desired **protection level**, select **Generated key** for Key material, select **Symmetric encrypt/decrypt** from the **Purpose** dropdown list, configure the key rotation parameters and labels. Choose **Create** to deploy the new Cloud KMS CMEK.
5. Once the new CMEK is available, navigate to the [Vertex AI Datasets console](https://console.cloud.google.com/vertex-ai/datasets).
6. Choose **Create**, and perform the following actions:
   - A. For **Dataset name**, provide a unique name for your new Vertex AI dataset.
   - B. For **Select a data type and objective**, choose the type of data your dataset will contain and select an objective.
   - C. For **Region**, select the GCP location where the new dataset will be deployed. This must match the region of the CMEK created in step 4.
   - D. Choose **Advanced Options** to display the encryption configuration settings. For **Encryption**, choose **Cloud KMS key**, and select the CMEK created in step 4. Inside the service account permissions notice box, choose **GRANT** to grant the service account the `cloudkms.cryptoKeyEncrypterDecrypter` IAM role on the selected CMEK.
   - E. Choose **Create** to create your new Vertex AI dataset.
7. Import the data backed up in step 3 into the newly created CMEK-encrypted dataset.
   - A. Click on the name (link) of the newly created dataset to access the dataset details page.
   - B. Choose the **Import** tab, then choose **Select import files from Cloud Storage**, and provide the path to the exported data from step 3.
   - C. Choose **Continue** to start the import operation. Monitor the import process to ensure it completes successfully.
8. Once the data import is verified, delete the original non-CMEK-encrypted dataset.
9. Repeat steps 6–8 for each Vertex AI dataset that you want to re-create in the selected GCP project.
10. Repeat steps 2–9 for each project deployed within your Google Cloud account.

## Verification Steps

#### Verify Vertex AI Dataset Encryption Protection Level

1. Sign in to the [Google Cloud Management Console](https://console.cloud.google.com/).
2. Select the GCP project you want to examine from the console top navigation bar.
3. Navigate to the [Vertex AI Datasets console](https://console.cloud.google.com/vertex-ai/datasets) and click on the name (link) of the Vertex AI dataset that you want to examine.
4. On the dataset details page, Select **Analyze** tab to check the **Encryption type** attribute value listed under **Properties**. If **Encryption type** is set to **Cloud KMS**, the dataset is correctly encrypted with CMEK.
5. Repeat steps 3–4 for each Vertex AI dataset in the selected GCP project.
6. Repeat steps 2–5 for each project deployed within your Google Cloud account.
