
# DynamoDBExportAbsoluteCapacity
### About this example

DynamoDB on demand capacity tables should be exportable with an absolute amount of read capacity units targeted per second. In order to accomplish this, we must set the `dynamodb.throughput.read` in hive in lieu of the usual *read.ratio* setting. This definition will work on provisioned or on demand tables, and it runs on emr 5.29.

### To use


1. Upload exportDynamoDBTableToS3Absolute to your own S3 bucket with the following AWS CLI command `aws s3 cp exportDynamoDBTableToS3Absolute s3://YOURBUCKETHERE/exportDynamoDBTableToS3Absolute --acl public-read `
 - Ensure your bucket allows objects to be written with a public ACL. If this is restricted, leave out the ACL make sure your pipeline role as defined in the template has getobject access to the key.
1. Create the data pipeline with the definition
1. Fill in the values for each parameter (below)
1. Schedule and activate the pipeline

#### Parameters
1. `myOutputS3Loc` - S3 location prefixed by s3:// where the data should be put. When the data is exported, it will be under a 'folder' with this prefix
1. `myDDBTableName` - DDB table name
1. `myDDBReadThroughput` - **Absolute number of read capacity units** for the export. This is not a read ratio, this is the literal amount the tool should try to consume. Often the EMR cluster will fall behind the target throughput systematically in first 5 minutes of its runtime. You should increase the target read throughput value until you get a real-world consumed value that meets your requirements. Don't put in 1000 (RCU) and expect the cluster to hit that many units out of the box.
1. `myDDBRegion` - friendly region name. e.g. us-east-1
1. `myLogUri` - S3 path for the logs from the job. This is passed to `pipelineLogUri`
1. `s3ScriptLocation` - Full S3 script location where you uploaded the *exportDynamoDBTableToS3Absolute* script.
