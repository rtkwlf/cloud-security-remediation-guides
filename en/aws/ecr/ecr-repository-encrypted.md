# AWS / ECR / ECR Repository Encrypted

## Quick Info

| | |
|-|-|
| **Plugin Title** | ECR Repository Encrypted |
| **Cloud** | AWS |
| **Category** | ECR |
| **Description** | Ensures the images in ECR repository are encrypted using desired encryption level |
| **More Info** | By default, Amazon ECR uses server-side encryption with Amazon S3-managed encryption keys which encrypts your data at rest using an AES-256 encryption algorithm. Use customer-managed keys instead, in order to gain more granular control over encryption/decryption process. |
| **AWS Link** | https://docs.aws.amazon.com/AmazonECR/latest/userguide/encryption-at-rest.html |
| **Recommended Action** | Create ECR Repository with customer-manager keys (CMKs). |

## Detailed Remediation Steps

**Note:** ECR repository encryption configuration cannot be changed after the repository has been created. You will need to create a new repository with the desired encryption settings and migrate your images.

1. Log in to the AWS Management Console.
2. Select the "Services" option and search for KMS.
3. Scroll down the left navigation panel and choose "Customer managed keys" under "Key Management Service".
4. Click on the "Create key" button at the top of the page.
5. On the "Configure key" page, select "Symmetric" as the key type. Under "Advanced options", select "KMS" for Key material origin and "Single-Region key" for Regionality. Click "Next".
6. On the "Add labels" page, provide an "Alias" (e.g., "ecr-encryption-key") and a "Description" for the KMS key. Click "Next".
7. On the "Define key administrative permissions" page, select the IAM users and roles who can administer the KMS key. Click "Next".
8. On the "Define key usage permissions" page, select the IAM users and roles that can use the key to encrypt and decrypt ECR repositories. Click "Next".
9. On the "Review and edit key policy" page, review the key policy configuration and click "Finish" to create the KMS key.
10. Copy the KMS key ARN from the key details page for use in the next section.
11. Navigate to the "Services" menu and search for ECR (Elastic Container Registry).
12. In the left navigation panel, select "Repositories" under "Private registry".
13. Click on the "Create repository" button.
14. On the "Create repository" page, select "Private" for repository visibility.
15. Enter a "Repository name" under the "General settings" section.
16. Scroll down to the "Encryption settings" section and perform the following:
   a. Select "KMS" as the encryption type.
   b. Under "KMS key", select "Customize encryption settings (advanced)".
   c. Select the customer-managed KMS key created in the previous steps from the dropdown menu, or paste the KMS key ARN.
17. Configure any additional settings as needed (such as image scanning, tag immutability, etc.) and click "Create repository".
18. If you have an existing repository with images that need to be migrated, perform the following steps:
   a. Authenticate Docker to your ECR registry using the AWS CLI command: `aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <account-id>.dkr.ecr.<region>.amazonaws.com`
   b. Pull the images from the old repository using Docker: `docker pull <old-repository-uri>:<tag>`
   c. Tag the images with the new repository URI: `docker tag <old-repository-uri>:<tag> <new-repository-uri>:<tag>`
   d. Push the images to the new encrypted repository: `docker push <new-repository-uri>:<tag>`
19. Update any applications, CI/CD pipelines, or services to reference the new encrypted repository URI.
20. After verifying that all images have been successfully migrated and applications are working correctly, delete the old repository by selecting it from the repositories list and clicking "Delete".
21. Repeat steps number 2 - 20 to enable encryption for all other ECR repositories.