# AWS / SecurityHub / Security Hub Enabled

## Quick Info

| | |
|-|-|
| **Plugin Title** | Security Hub Enabled |
| **Cloud** | AWS |
| **Category** | SecurityHub |
| **Description** | Ensure that AWS Security Hub is enabled. |
| **More Info** | AWS Security Hub provides a comprehensive view of your security posture across your AWS accounts. It aggregates, organises, and prioritises security findings from various AWS services. |
| **AWS Link** | https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-settingup.html |
| **Recommended Action** | Enable AWS Security Hub for enhanced security monitoring and compliance. |

## Detailed Remediation Steps
1. Log in to the AWS Management Console.
2. Select the "Services" option and search for "Security Hub".
3. When you open the Security Hub console for the first time, click on "Go to Security Hub".
4. On the welcome page, you'll see the "Security standards" section listing the security standards that Security Hub supports (such as AWS Foundational Security Best Practices, CIS AWS Foundations Benchmark, and PCI DSS).
5. Select the checkbox for each standard you want to enable. You can enable or disable standards and individual controls at any time later.
6. Click "Enable Security Hub" to complete the setup.
7. Security Hub will now begin monitoring your AWS environment and generating security findings based on the enabled standards.
8. Repeat steps 2-7 for each AWS Region where you want to enable Security Hub to ensure comprehensive security monitoring across all regions.

