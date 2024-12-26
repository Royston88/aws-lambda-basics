# aws-lambda-basics

## 1. Purpose of the execution role on the Lambda function:

The execution role on the Lambda function defines the permissions that the Lambda function has when it is executed. This role grants Lambda the necessary permissions to interact with other AWS resources during its execution. For example, if the Lambda function needs to read from or write to an S3 bucket, the execution role should have permissions for `s3:GetObject` (to read from S3) or `s3:PutObject` (to upload files to S3). The execution role is associated with Lambda and is used during the execution of the function to interact with resources like S3, DynamoDB, or other AWS services.

## 2. Purpose of the resource-based policy on the Lambda function:

A resource-based policy on the Lambda function defines who can invoke the Lambda function. Unlike the execution role, which grants permissions to the Lambda function, the resource-based policy specifies which AWS services or principals (e.g., IAM users, roles, or other services) are allowed to invoke the Lambda function. For example, in the case of an S3 event trigger, the S3 bucket will need permission to invoke the Lambda function when new files are uploaded. This is achieved through the Lambda function’s resource-based policy.

## 3. Changes needed to allow the Lambda function to upload a file into an S3 bucket:

### Update on the execution role:

The execution role associated with the Lambda function needs the necessary permissions to interact with S3, specifically write permissions (i.e., to upload files to the S3 bucket). This can be achieved by attaching the following policy to the execution role:
`s3:PutObject`: This permission allows the Lambda function to upload objects to a specified S3 bucket.

Example:
``` json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}
```

### Resource-based policy for Lambda:

The new resource-based policy for the Lambda function should allow S3 to invoke the Lambda function upon specific events, such as the creation of a file in an S3 bucket.

Example:
``` json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "lambda:InvokeFunction",
      "Principal": "s3.amazonaws.com",
      "SourceArn": "arn:aws:s3:::your-bucket-name",
      "SourceAccount": "your-aws-account-id"
    }
  ]
}
```

