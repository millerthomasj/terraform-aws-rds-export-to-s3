# Terraform Module: RDS Snapshots Export To S3

## Brief
Terraform module that deploys Lambda functions to trigger exports of RDS snapshots to S3.

## Design

<div align="left">
  <img src="https://raw.githubusercontent.com/binbashar/terraform-aws-rds-export-to-s3/master/assets/rds-export-to-s3.png" alt="leverage" width="400"/>
</div>

## Requirements

No requirements.

## Providers

| Name | Version |
|------|---------|
| <a name="provider_aws"></a> [aws](#provider\_aws) | n/a |

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_monitor_export_task_lambda"></a> [monitor\_export\_task\_lambda](#module\_monitor\_export\_task\_lambda) | github.com/terraform-aws-modules/terraform-aws-lambda | v2.7.0 |
| <a name="module_start_export_task_lambda"></a> [start\_export\_task\_lambda](#module\_start\_export\_task\_lambda) | github.com/terraform-aws-modules/terraform-aws-lambda | v2.7.0 |

## Resources

| Name | Type |
|------|------|
| [aws_cloudwatch_event_rule.rdsSnapshotCreation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudwatch_event_rule) | resource |
| [aws_cloudwatch_event_target.rdsSnapshotCreationTopic](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudwatch_event_target) | resource |
| [aws_iam_policy.rdsStartExportTaskLambda](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_policy) | resource |
| [aws_iam_role.rdsSnapshotExportTask](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role) | resource |
| [aws_iam_role_policy.rdsSnapshotExportToS3](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy) | resource |
| [aws_kms_alias.snapshotExportEncryptionKey](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/kms_alias) | resource |
| [aws_kms_key.snapshotExportEncryptionKey](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/kms_key) | resource |
| [aws_lambda_permission.snsCanTriggerMonitorExportTask](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/lambda_permission) | resource |
| [aws_lambda_permission.snsCanTriggerStartExportTask](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/lambda_permission) | resource |
| [aws_sns_topic.rdsSnapshotsEvents](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/sns_topic) | resource |
| [aws_sns_topic_policy.default](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/sns_topic_policy) | resource |
| [aws_sns_topic_subscription.lambdaRdsSnapshotToS3Exporter](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/sns_topic_subscription) | resource |
| [aws_sns_topic_subscription.lambdaRdsSnapshotToS3Monitor](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/sns_topic_subscription) | resource |
| [aws_caller_identity.current](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/caller_identity) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_database_name"></a> [database\_name](#input\_database\_name) | The name of the database whose snapshots we want to export to S3. | `string` | `null` | no |
| <a name="input_log_level"></a> [log\_level](#input\_log\_level) | The log level of the Lambda function. | `string` | `"INFO"` | no |
| <a name="input_notifications_topic_arn"></a> [notifications\_topic\_arn](#input\_notifications\_topic\_arn) | The ARN of an SNS Topic which will be used for publishing notifications messages. | `string` | `null` | no |
| <a name="input_prefix"></a> [prefix](#input\_prefix) | Prefix that will be used for naming resources. | `string` | `null` | no |
| <a name="input_rds_event_id"></a> [rds\_event\_id](#input\_rds\_event\_id) | RDS (CloudWatch) Event ID that will trigger the calling of RDS Start Export Task API:<br>- Automated snapshots of Aurora RDS: RDS-EVENT-0169<br>- Automated snapshots of non-Aurora RDS: RDS-EVENT-0091<br>Only automated backups of either RDS Aurora and RDS non-Aurora are supported.<br>Ref: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_Events.Messages.html#USER_Events.Messages.snapshot | `string` | n/a | yes |
| <a name="input_snapshots_bucket_arn"></a> [snapshots\_bucket\_arn](#input\_snapshots\_bucket\_arn) | The ARN of the bucket where the RDS snapshots will be exported to. | `string` | `null` | no |
| <a name="input_snapshots_bucket_name"></a> [snapshots\_bucket\_name](#input\_snapshots\_bucket\_name) | The name of the bucket where the RDS snapshots will be exported to. | `string` | `null` | no |
| <a name="input_tags"></a> [tags](#input\_tags) | (Optional) A mapping of tags to assign to the bucket. | `map(string)` | `{}` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_monitor_export_task_lambda_function_arn"></a> [monitor\_export\_task\_lambda\_function\_arn](#output\_monitor\_export\_task\_lambda\_function\_arn) | Start Export Task Monitor Lambda Function ARN |
| <a name="output_monitor_export_task_lambda_role_arn"></a> [monitor\_export\_task\_lambda\_role\_arn](#output\_monitor\_export\_task\_lambda\_role\_arn) | Start Export Task Monitor Lambda Role ARN |
| <a name="output_snapshots_events_sns_topics_arn"></a> [snapshots\_events\_sns\_topics\_arn](#output\_snapshots\_events\_sns\_topics\_arn) | RDS Snapshots Events SNS Topics ARN |
| <a name="output_snapshots_export_encryption_key_arn"></a> [snapshots\_export\_encryption\_key\_arn](#output\_snapshots\_export\_encryption\_key\_arn) | Snapshots Export Encryption Key ARN |
| <a name="output_start_export_task_lambda_function_arn"></a> [start\_export\_task\_lambda\_function\_arn](#output\_start\_export\_task\_lambda\_function\_arn) | Start Export Task Lambda Function ARN |
| <a name="output_start_export_task_lambda_role_arn"></a> [start\_export\_task\_lambda\_role\_arn](#output\_start\_export\_task\_lambda\_role\_arn) | Start Export Task Lambda Role ARN |