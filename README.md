# AWS Restore Database Point-in-time
## Restore AWS RDS to Point-in-time using AWS Lambda Function


![Architecture Diagrams](https://user-images.githubusercontent.com/47545538/187001901-2e7f7b0f-de8c-460c-9920-38434c038599.jpg)
Use the AWS CloudFormation template to deploy the AWS Lambda function to restore the Amazon RDS DB instances to point-in-time using Automated backup point-in-time-recovery (PITR) functionality, where you can restore to any point in time within your backup retention period.

For more information, visit [Restoring a DB instance to a specified time](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_PIT.html).
___
### Process Overview:
* AWS Lambda uses Python 3.9 runtime and AWS SDK for Python (Boto3).
* Use Lambda function payload to specify source DB instance identifier from which to restore and target DB instance identifier to be created.
<pre><code>{
    "RestoreDBInstanceIdentifier": "<b><i>source_db_instance_identifier</b></i>",
    "DBInstanceIdentifier": "<b><i>target_db_instance_identifier</b></i>"
}</pre></code>
* The Lambda function checks whether or not the DB instance exists with the target DB instance identifier. If it exists, the Lambda function modifies the DB instance identifier with a random suffix, creates a final DB snapshot, and deletes the DB instance.
* The Lambda function restores the DB instance to point-in-time with the latest restoration time using the source DB instance parameters configuration.

___
### Use Cases:
* Restore the DB instance for other environments with the latest data.
* Perform scheduled jobs for processes that require data manipulation with the latest data without impacting the original DB instance.
* Perform ad hoc tasks temporarily with the latest data.
___
You can invoke the Lambda function to restore the DB instance using Console, AWS CLI, Amazon EventBridge, Function URL, etc. For more information, visit [Invoking Lambda functions](https://docs.aws.amazon.com/lambda/latest/dg/lambda-invocation.html).

In the AWS Lambda function, Boto3 restore_db_instance_to_point_in_time method parameters refer to Amazon RDS for MySQL as a source DB instance identifier from which to restore. Further, you can modify the function configuration and programming logic for different DB engines accordingly. For more information on Boto3 restore DB instance to point-in-time, visit [restore_db_instance_to_point_in_time](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html#RDS.Client.restore_db_instance_to_point_in_time).
