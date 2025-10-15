# AWS / AI & ML / Fraud Detector Data Encrypted

## Quick Info

| | |
|-|-|
| **Plugin Title** | Fraud Detector Data Encrypted |
| **Cloud** | AWS |
| **Category** | AI & ML |
| **Description** | Ensures Amazon Fraud Detector has encryption enabled for data at rest with desired KMS encryption level |
| **More Info** | Amazon Fraud Detector encrypts your data at rest with AWS-managed KMS key. Use customer-managed KMS keys (CMKs) instead in order to follow your organization's security and compliance requirements. |
| **AWS Link** | https://docs.aws.amazon.com/frauddetector/latest/ug/encryption-at-rest.html |
| **Recommended Action** | Enable encryption for data at rest using PutKMSEncryptionKey API. |

## Detailed Remediation Steps
**Note:** Amazon Fraud Detector supports different encryption levels (from lowest to highest): `awskms` (AWS-managed KMS key - default), `awscmk` (customer-managed KMS key), `externalcmk` (customer-managed externally sourced KMS key), and `cloudhsm` (customer-managed CloudHSM sourced KMS key). By default, Fraud Detector uses AWS-managed encryption. The following steps show how to configure customer-managed KMS key encryption (`awscmk` level) to meet higher security and compliance requirements. Your organization's security policy may require a specific encryption level.

1. Log in to the AWS Management Console.
2. Select the "Services" option and search for KMS. </br> 
3. Scroll down the left navigation panel and choose "Customer managed keys" under "Key Management Service".</br>
4. Click on the "Create key" button at the top panel to create a new customer-managed KMS key.</br>
5. On the "Configure key" page, select "Symmetric" as the key type. Under "Advanced options", select "KMS" for "Key material origin" and "Single-Region key" for "Regionality" (Amazon Fraud Detector does not support multi-Region KMS keys). Click "Next" button.</br>
6. On the "Add labels" page, provide an "Alias" (e.g., "fraud-detector-encryption-key") and "Description" for the new KMS key. Click on the "Next" button.
7. On the "Define key administrative permissions" page, select the IAM users and roles who can administer the new KMS key through the KMS API. Click on the "Next" button.
8. On the "Define key usage permissions" page, select the IAM users and roles that can use the KMS key to encrypt and decrypt data. Make sure to add the necessary permissions for the Amazon Fraud Detector service. Click on the "Next" button.
9. On the "Review and edit key policy" page, add the following statement to the key policy to grant Amazon Fraud Detector permissions to use the key:
```json
{
    "Effect": "Allow",
    "Principal": {
        "Service": "frauddetector.amazonaws.com"
    },
    "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt*",
        "kms:GenerateDataKey*",
        "kms:DescribeKey",
        "kms:CreateGrant",
        "kms:RetireGrant"
    ],
    "Resource": "*"
}
```
10. Review the complete key policy and click on the "Finish" button to create the KMS key.
11. Once the key is created, copy the KMS key ARN from the key details page (e.g., arn:aws:kms:us-east-1:123456789012:key/12345678-1234-1234-1234-123456789012).
12. Open AWS CloudShell or your local AWS CLI configured with appropriate credentials.
13. Run the following AWS CLI command to configure Amazon Fraud Detector to use the customer-managed KMS key:
```bash
aws frauddetector put-kms-encryption-key --kms-encryption-key-arn arn:aws:kms:REGION:ACCOUNT-ID:key/KEY-ID --region REGION
```
Replace `REGION` with your AWS region (e.g., us-east-1), `ACCOUNT-ID` with your AWS account ID, and `KEY-ID` with your KMS key ID.</br>
14. Verify the encryption configuration by running the following command:</br>
```bash
aws frauddetector get-kms-encryption-key --region REGION
```
15. The response should show your customer-managed KMS key ARN, confirming that encryption is now configured. Note: The encryption configuration applies at the account level for all Fraud Detector resources in the region, not per individual detector.</br>
16. To verify Fraud Detector resources exist in the region, run:</br>
```bash
aws frauddetector get-detectors --region REGION
```
17. Important: Data generated after setting up the customer-managed KMS key will be encrypted with the new key. Data generated before this configuration will remain encrypted with the previous encryption settings.</br>
18. Repeat steps 2-17 for other AWS regions where Amazon Fraud Detector is deployed. Verify Fraud Detector usage per region before configuring encryption.</br>