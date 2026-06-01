# GOOGLE / AI & ML / Vertex AI Model Encryption

## Quick Info

| | |
|-|-|
| **Plugin Title** | Vertex AI Model Encryption |
| **Cloud** | GOOGLE |
| **Category** | AI & ML |
| **Description** | Ensure that Vertex AI models are encrypted using desired encryption protection level. |
| **More Info** | By default Google encrypts all models using Google-managed encryption keys. To have more control over the encryption process of your Vertex AI models you can use Customer-Managed Keys (CMKs). |
| **GOOGLE Link** | https://cloud.google.com/vertex-ai/docs/general/cmek |
| **Recommended Action** | Recreate existing models with desired protection level. |

---

## Introduction

This guide addresses the compliance requirement to encrypt Vertex AI models with Customer-Managed Encryption Keys (CMEKs) at the desired protection level.

## Prerequisites

#### Required IAM Roles and Permissions

- Cloud KMS Admin role (roles/cloudkms.admin) to manage encryption keys.
- Cloud KMS CryptoKey Encrypter/Decrypter role (roles/cloudkms.cryptoKeyEncrypterDecrypter) for the Vertex AI service agent.
- Vertex AI Admin or equivalent permissions to delete and recreate models.

## Remediation Steps

> ⚠️ Warning: Remediating non-compliant Vertex AI models requires deleting existing models and recreating them with CMEK encryption. This process may cause service disruption if models are currently in use for inference or training. Ensure models are backed up and dependent applications are prepared for downtime during the recreation process.

#### Enable Cloud KMS API and Create Key Ring

1. Navigate to the **Key Management** page in the Google Cloud Console.
2. Click **Create key ring**. </br> <img src="/resources/google/ai&ml/vertex-ai-model-encryption/step1.png"/>
3. Enter a name for your key ring in the **Key ring name** field.
4. Select a location type (Region or Multi-region) and  specific region (e.g., 'us-east1') that is geographically near your resources.
5. Click **Create**. </br> <img src="/resources/google/ai&ml/vertex-ai-model-encryption/step2.png"/>

#### Create CMEK with Desired Protection Level

1. In the Google Cloud Console, go to the **Key Management** page.
2. Click the name of the key ring where you will create a key. </br> <img src="/resources/google/ai&ml/vertex-ai-model-encryption/step3.png"/>
3. Click **Create key**. </br> <img src="/resources/google/ai&ml/vertex-ai-model-encryption/step4.png"/>
4. Enter a name for your key in the **Key name** field.
5. For **Protection level**, select **Software**, **HSM**, or **Single-tenant HSM** based on your compliance requirements.
6. For **Key material**, select **Generated key**.
7. For **Purpose**, select **Symmetric encrypt/decrypt**.
8. Set the values for **Rotation period** and **Starting on** based on your compliance requirements.
9. Click **Create**. </br> <img src="/resources/google/ai&ml/vertex-ai-model-encryption/step5.png"/>

#### Delete Non-Compliant Vertex AI Models

1. In the Google Cloud Console, go to the **Vertex AI** page.
2. Click **Models** > **Model Registry** in the Google Cloud Console. </br> <img src="/resources/google/ai&ml/vertex-ai-model-encryption/step6.png"/>
3. Identify all models that do not meet the desired CMEK protection level requirement.
4. Select More actions from each model you want to delete and click the **Delete** button. When you delete the model, all associated model versions and evaluations are deleted from your Google Cloud project.
5. Confirm the deletion when prompted.

#### Recreate Vertex AI Models with CMEK Encryption

1. Navigate to **Vertex AI** page.
2. Click **Models** > **Model Registry** in the Google Cloud Console. </br> <img src="/resources/google/ai&ml/vertex-ai-model-encryption/step6.png"/>
3. Configure the model settings as required to create a model or import a model.
4. If you are creating a model, click **Create** model. Under **Model details**, click **Advanced options**. In the **Encryption** section, select **Cloud KMS Key** > **Cloud KMS**, and then select a Cloud KMS key that you created earlier. </br> <img src="/resources/google/ai&ml/vertex-ai-model-encryption/step7.png"/> <br/> <img src="/resources/google/ai&ml/vertex-ai-model-encryption/step8.png"/>
5. If you are importing a model, click **Import** model. Under **Name and region**, click **Advanced options**. In the **Encryption** section, select **Cloud KMS Key** > **Cloud KMS**, and then select a Cloud KMS key that you created earlier. </br> <img src="/resources/google/ai&ml/vertex-ai-model-encryption/step9.png"/> <br/> <img src="/resources/google/ai&ml/vertex-ai-model-encryption/step10.png"/>
6. Complete the model creation or import process.
7. Verify that the model is now encrypted with the desired CMEK protection level.