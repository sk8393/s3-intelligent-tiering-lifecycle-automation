# S3 Intelligent-Tiering Lifecycle Automation

## Description
[Amazon Simple Storage Service (S3) Intelligent-Tiering](https://aws.amazon.com/s3/storage-classes/intelligent-tiering/) is designed to optimize storage costs by automatically moving data between multiple access tiers.  Less frequently accessed objects will be moved to colder tier which has smaller per Gigabyte storage cost.

S3 Intelligent-Tiering deserves to be the default storage class when using S3.  Because the default three access tiers, 1/ **Frequent Access Tier**, 2/ **Infrequent Access Tier**, and 3/ **Archive Instant Access Tier**, provide low latency access to the objects, and there is no fee for moving objects between tiers and retrieval of the data.  Just to be sure, there is a monitoring fee for objects bigger than 128 Kilobytes ($0.0025 per 1,000 objects).

If you are considering migration of objects stored in **Standard** storage class to **Intelligent-Tiering**, Lifecycle can automatically transition objects from **Standard** to **Intelligent-Tiering** by set rule. 
 Transitioning objects from **Standard** to **Intelligent-Tiering** through Lifecycle will be charged $0.01 per 1,000 requests.  Filter can be set to Lifecycle rule as objects bigger than 128 Kilobytes are only transitioned.

## Installation
This sample can be deployed via CloudFormation template.  Applying Lifecycle rule only happens for S3 bucket which has specific tag.  Default is `'apply-lifecycle-for-intelligent-tiering':'true'`.  You can change these tag key/value as CloudFormation parameter.  Following parameters can be set through CloudFormation.

![CloudFormation_parameter](https://github.com/sk8393/s3-intelligent-tiering-lifecycle-automation/assets/13175031/5b30c870-4ecf-4478-949f-d46197d5697e)

## Usage
System architecture looks like below.  Lifecycle rule will be applied only when S3 bucket has specific tag.  [AWS Config](https://aws.amazon.com/config/) is a regional service, but this solution can cover all S3 buckets across different regions.  Please enable Config on the region where you deploy CloudFormation stack beforehand.

![S3_Intelligent-Tiering_Lifecycle_Automation](https://github.com/sk8393/s3-intelligent-tiering-bucket-policy-enforcement/assets/13175031/d7ab053c-4240-4469-844c-fb31fd7059c6)

Lifecycle rule to delete incomplete multipart uploads (after 7 days) will be also applied along with transition rule.  In case you want to apply multiple Lifecylce rules at once, please refer the Ptyhon code.

## Support
This sample was tested that the expected results were obtained in an actual AWS account.  It is implemented so that changes are made when only specific tag is set to S3 bucket, but we recommend that you try it once in a test environment when using it.
