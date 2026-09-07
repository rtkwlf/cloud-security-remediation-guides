# AWS / DocumentDB / DocumentDB Cluster Instance Certificate Rotation

## Quick Info

| | |
|-|-|
| **Plugin Title** | DocumentDB Cluster Instance Certificate Rotation |
| **Cloud** | AWS |
| **Category** | DocumentDB |
| **Description** | Ensure that DocumentDB cluster instance certificates are rotated. |
| **More Info** | AWS DocumentDB cluster certificate rotation ensures that cluster\ |
| **AWS Link** | https://docs.aws.amazon.com/documentdb/latest/developerguide/ca_cert_rotation.html |
| **Recommended Action** | Modify DocumentDB cluster instance and rotate the old server certificate. |

---

## Introduction

This guide provides step-by-step instructions to rotate the server certificate on Amazon DocumentDB cluster instances. Certificate rotation ensures continuous protection for data in transit and prevents connectivity issues when certificates expire.

> ⚠️ **Warning:** Certificate rotation may cause temporary connectivity disruption if applications are not updated with the new CA certificate bundle first. Ensure applications are updated before rotating the server certificate to avoid service interruption.

## Remediation Steps

#### Identify DocumentDB Instances Requiring Certificate Rotation

1. Sign in to the AWS Management Console, and open the Amazon DocumentDB console at [https://console.aws.amazon.com/docdb](https://console.aws.amazon.com/docdb). </br> <img src="/resources/aws/documentdb/documentdb-cluster-instance-certificate-rotation/step1.png"/>
2. In the list of Regions in the upper-right corner of the screen, choose the AWS Region in which your instances reside. </br> <img src="/resources/aws/documentdb/documentdb-cluster-instance-certificate-rotation/step2.png"/>
3. In the navigation pane on the left side of the console, choose **Clusters**. </br> <img src="/resources/aws/documentdb/documentdb-cluster-instance-certificate-rotation/step3.png"/>
4. Review the **Certificate authority** column (near the far right of the table) to identify instances still using the old server certificate (`rds-ca-2019`). </br> <img src="/resources/aws/documentdb/documentdb-cluster-instance-certificate-rotation/step4.png"/>

#### Modify DocumentDB Instance to Rotate Server Certificate

1. Sign in to the AWS Management Console, and open the Amazon DocumentDB console at [https://console.aws.amazon.com/docdb](https://console.aws.amazon.com/docdb).
2. In the navigation pane, choose **Clusters**.
3. In the Clusters navigation box, locate the **Cluster Identifier** column; your instances are listed under clusters.
4. Check the box to the left of the instance you wish to modify. </br> <img src="/resources/aws/documentdb/documentdb-cluster-instance-certificate-rotation/step-4.png"/>
5. Choose **Actions**, and then choose **Modify**. </br> <img src="/resources/aws/documentdb/documentdb-cluster-instance-certificate-rotation/step-5.png"/>
6. In the **Modify instance** pane, under **Certificate authority**, select a new server certificate (`rds-ca-rsa2048-g1`, `rds-ca-rsa4096-g1`, or `rds-ca-ecc384-g1`). </br> <img src="/resources/aws/documentdb/documentdb-cluster-instance-certificate-rotation/step-6.png"/>
7. Choose **Continue** to see a summary of your changes. </br> <img src="/resources/aws/documentdb/documentdb-cluster-instance-certificate-rotation/step-7.png"/>
8. Under **Scheduling of modifications**, choose to apply the modification immediately or during the next maintenance window.
9. Choose **Modify instance** to complete the update. </br> <img src="/resources/aws/documentdb/documentdb-cluster-instance-certificate-rotation/step-9.png"/>
