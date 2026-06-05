# AWS / AI & ML / Custom Model Encryption Enabled

## Quick Info

| | |
|-|-|
| **Plugin Title** | Custom Model Encryption Enabled |
| **Cloud** | AWS |
| **Category** | AI & ML |
| **Description** | Ensure that an Amazon Bedrock custom models are encrypted with desired encryption level. |
| **More Info** | When you encrypt AWS Bedrock custom model using your own AWS Customer Managed Keys (CMKs) for enhanced protection, you have full control over who can use the encryption keys to access your custom model. |
| **AWS Link** | https://docs.aws.amazon.com/bedrock/latest/userguide/encryption-custom-job.html |
| **Recommended Action** | Encrypt Bedrock custom model with desired encryption level. |

---

## Introduction

This rule checks that every Amazon Bedrock custom model is encrypted at rest using a KMS key that meets or exceeds a configurable desired encryption level (`awskms`, `awscmk`, `externalcmk`, or `cloudhsm`). Because the encryption key is set immutably at job-creation time via `customModelKmsKeyId`, a non-compliant model cannot be re-encrypted in place and must be deleted and recreated with the correct customer managed KMS key.

## Remediation Steps

#### Create a Customer Managed KMS Key and Attach Key Policy

1. Open the AWS KMS console at [https://console.aws.amazon.com/kms](https://console.aws.amazon.com/kms). </br> <img src="/resources/aws/ai&ml/custom-model-encryption-enabled/step1.png"/>
2. Use the **Region selector** in the upper-right corner to select the same AWS Region as your Bedrock custom model. </br> <img src="/resources/aws/ai&ml/custom-model-encryption-enabled/step2.png"/>
3. In the navigation pane, choose **Customer managed keys**. </br> <img src="/resources/aws/ai&ml/custom-model-encryption-enabled/step3.png"/>
4. Choose **Create key**. </br> <img src="/resources/aws/ai&ml/custom-model-encryption-enabled/step4.png"/>
5. For **Key type**, choose **Symmetric**. The **Encrypt and decrypt** key usage option is selected automatically. </br> <img src="/resources/aws/ai&ml/custom-model-encryption-enabled/step5.png"/>
6. Choose **Next**.
7. Enter an **Alias** for the key (the alias name cannot begin with `aws/`). </br> <img src="/resources/aws/ai&ml/custom-model-encryption-enabled/step7.png"/>
8. Optionally enter a **Description** and any **Tags**, then choose **Next**.
9. In **Key administrators**, select the IAM users and roles that will administer this key, then choose **Next**. </br> <img src="/resources/aws/ai&ml/custom-model-encryption-enabled/step9.png"/>
10. In **Key users**, select the IAM users and roles that will use this key in cryptographic operations, then choose **Next**. </br> <img src="/resources/aws/ai&ml/custom-model-encryption-enabled/step10.png"/>
11. On the **Review** page, choose **Edit** in the key policy section and replace the policy with the following, substituting your account ID, region, and Bedrock service role ARN:

```json
{
    "Version": "2012-10-17",
    "Id": "PermissionsCustomModelKey",
    "Statement": [
        {
            "Sid": "Enable IAM User Permissions",
            "Effect": "Allow",
            "Principal": {"AWS": "arn:aws:iam::111122223333:root"},
            "Action": "kms:*",
            "Resource": "*"
        },
        {
            "Sid": "PermissionsEncryptCustomModel",
            "Effect": "Allow",
            "Principal": {"AWS": ["arn:aws:iam::111122223333:role/MyCustomizationRole"]},
            "Action": ["kms:Decrypt", "kms:GenerateDataKey", "kms:DescribeKey", "kms:CreateGrant"],
            "Resource": "*",
            "Condition": {
                "StringLike": {"kms:ViaService": ["bedrock.us-east-1.amazonaws.com"]}
            }
        }
    ]
}
```
</br> <img src="/resources/aws/ai&ml/custom-model-encryption-enabled/step11.png"/>

12. Choose **Finish** to create the KMS key. Record the resulting **Key ARN** for use in subsequent steps.

#### Delete the Non-Compliant Custom Model

> ⚠️ **Warning** 
> Deleting a custom model is irreversible. All training data, hyperparameters, and job configuration must be recorded before deletion so the model can be recreated.

1. Record the non-compliant model's name, base model identifier, training data S3 URI, output S3 URI, hyperparameters, and service role ARN before proceeding.
2. Run the following command to delete the non-compliant model, replacing `<model-name-or-arn>` with the model's name or ARN:

```bash
aws bedrock delete-custom-model \
  --model-identifier <model-name-or-arn>
```

3. Confirm the command returns HTTP 200 with no output, indicating successful deletion.

#### Create a New Model Customization Job with KMS Encryption

1. Open the Amazon Bedrock console at [https://console.aws.amazon.com/bedrock](https://console.aws.amazon.com/bedrock). </br> <img src="/resources/aws/ai&ml/custom-model-encryption-enabled/step-1.png"/>
2. In the left navigation pane, choose **Custom models** under **Tune**. </br> <img src="/resources/aws/ai&ml/custom-model-encryption-enabled/step-2.png"/>
3. In the **Models** tab, choose **Customize model** and then choose **Create Fine-tuning job**. </br> <img src="/resources/aws/ai&ml/custom-model-encryption-enabled/step-3.png"/>
4. In the **Model details** section, choose the base model you want to customize and enter a name for the resulting custom model.
5. Select **Model encryption** and choose the customer managed KMS key created in the previous section.
6. In the **Job configuration** section, enter a name for the job. </br> <img src="/resources/aws/ai&ml/custom-model-encryption-enabled/step-6.png"/>
7. In the **Input data** section, select the S3 location of the training dataset file and, if applicable, the validation dataset file. </br> <img src="/resources/aws/ai&ml/custom-model-encryption-enabled/step-7.png"/>
8. In the **Hyperparameters** section, enter the same hyperparameter values used by the original model.
9. In the **Output data** section, enter the Amazon S3 location where Amazon Bedrock should save the job output. </br> <img src="/resources/aws/ai&ml/custom-model-encryption-enabled/step-9.png"/>
10. In the **Service access** section, select the existing service role or create a new one.
11. Choose **Fine-tune model** to submit the job.
