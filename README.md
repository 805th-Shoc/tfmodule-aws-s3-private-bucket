Creates a private S3 bucket with good defaults:

- Private only objects
- Encryption
- Versioning
- Access logging
- Storage analytics

The following policy rules are set:

- Deny uploading public objects.
- Deny updating policy to allow public objects.

The following ACL rules are set:

- Retroactively remove public access granted through public ACLs
- Deny updating ACL to public

The following lifecycle rules are set:

- Incomplete multipart uploads are deleted after 14 days.
- Expired object delete markers are deleted.
- Noncurrent object versions transition to the Standard - Infrequent Access storage class after 30 days.
- Noncurrent object versions expire after 365 days.

## Terraform Versions

Terraform 0.13 and newer. Pin module version to ~> 3.X. Submit pull-requests to master branch.

Terraform 0.12. Pin module version to ~> 2.X. Submit pull-requests to terraform012 branch.

## Usage

```hcl
module "aws-s3-bucket" {
  source         = "trussworks/s3-private-bucket/aws"
  bucket         = "my-bucket-name"
  logging_bucket = "my-aws-logs"

  tags = {
    Name        = "My bucket"
    Environment = "Dev"
  }
}
```

<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
## Requirements

| Name | Version |
|------|---------|
| terraform | >= 0.13.0 |
| aws | >= 3.75.0 |

## Providers

| Name | Version |
|------|---------|
| aws | >= 3.75.0 |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [aws_s3_bucket.private_bucket](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket) | resource |
| [aws_s3_bucket_accelerate_configuration.private_bucket](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_accelerate_configuration) | resource |
| [aws_s3_bucket_acl.private_bucket](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_acl) | resource |
| [aws_s3_bucket_analytics_configuration.private_analytics_config](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_analytics_configuration) | resource |
| [aws_s3_bucket_cors_configuration.private_bucket](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_cors_configuration) | resource |
| [aws_s3_bucket_inventory.inventory](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_inventory) | resource |
| [aws_s3_bucket_lifecycle_configuration.private_bucket](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_lifecycle_configuration) | resource |
| [aws_s3_bucket_logging.private_bucket](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_logging) | resource |
| [aws_s3_bucket_policy.private_bucket](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_policy) | resource |
| [aws_s3_bucket_public_access_block.public_access_block](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_public_access_block) | resource |
| [aws_s3_bucket_server_side_encryption_configuration.private_bucket](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_server_side_encryption_configuration) | resource |
| [aws_s3_bucket_versioning.private_bucket](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket_versioning) | resource |
| [aws_caller_identity.current](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/caller_identity) | data source |
| [aws_iam_account_alias.current](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_account_alias) | data source |
| [aws_iam_policy_document.supplemental_policy](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_policy_document) | data source |
| [aws_partition.current](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/partition) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| abort\_incomplete\_multipart\_upload\_days | Number of days until aborting incomplete multipart uploads | `number` | `14` | no |
| bucket | The name of the bucket. | `string` | n/a | yes |
| bucket\_key\_enabled | Whether or not to use Amazon S3 Bucket Keys for SSE-KMS. | `bool` | `false` | no |
| cors\_rules | List of maps containing rules for Cross-Origin Resource Sharing. | `list(any)` | `[]` | no |
| custom\_bucket\_policy | JSON formatted bucket policy to attach to the bucket. | `string` | `""` | no |
| enable\_analytics | Enables storage class analytics on the bucket. | `bool` | `true` | no |
| enable\_bucket\_force\_destroy | If set to true, Bucket will be emptied and destroyed when terraform destroy is run. | `bool` | `false` | no |
| enable\_bucket\_inventory | If set to true, Bucket Inventory will be enabled. | `bool` | `false` | no |
| enable\_s3\_public\_access\_block | Bool for toggling whether the s3 public access block resource should be enabled. | `bool` | `true` | no |
| expiration | expiration blocks | `list(any)` | ```[ { "expired_object_delete_marker": true } ]``` | no |
| inventory\_bucket\_format | The format for the inventory file. Default is ORC. Options are ORC or CSV. | `string` | `"ORC"` | no |
| kms\_master\_key\_id | The AWS KMS master key ID used for the SSE-KMS encryption. | `string` | `""` | no |
| logging\_bucket | The S3 bucket to send S3 access logs. | `string` | `""` | no |
| noncurrent\_version\_expiration | Number of days until non-current version of object expires | `number` | `365` | no |
| noncurrent\_version\_transitions | Non-current version transition blocks | `list(any)` | ```[ { "days": 30, "storage_class": "STANDARD_IA" } ]``` | no |
| schedule\_frequency | The S3 bucket inventory frequency. Defaults to Weekly. Options are 'Weekly' or 'Daily'. | `string` | `"Weekly"` | no |
| sse\_algorithm | The server-side encryption algorithm to use. Valid values are AES256 and aws:kms | `string` | `"AES256"` | no |
| tags | A mapping of tags to assign to the bucket. | `map(string)` | `{}` | no |
| transfer\_acceleration | Whether or not to enable bucket acceleration. | `bool` | `null` | no |
| transitions | Current version transition blocks | `list(any)` | `[]` | no |
| use\_account\_alias\_prefix | Whether to prefix the bucket name with the AWS account alias. | `string` | `true` | no |
| use\_random\_suffix | Whether to add a random suffix to the bucket name. | `bool` | `false` | no |
| versioning\_status | A string that indicates the versioning status for the log bucket. | `string` | `"Enabled"` | no |

## Outputs

| Name | Description |
|------|-------------|
| arn | The ARN of the bucket. Will be of format arn:aws:s3:::bucketname. |
| bucket\_domain\_name | The bucket domain name. |
| bucket\_regional\_domain\_name | The bucket region-specific domain name. |
| id | The name of the bucket. |
| name | The Name of the bucket. Will be of format bucketprefix-bucketname. |
<!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->

