# AWS EC2 Password Change on Secret Rotation

This repository contains code that updates the password of a Windows EC2 instance when its secret is scheduled for rotation. The associated code repository contains the following:
1. This README. 

2. The source code for a lambda function. This function is invoked via Secrets Manager via the created rotation schedule in the CFN template password-rotation.yaml. It creates a new secret and updates the secret value, as well as update the password of the EC2 instance to match. 

3. A CloudFormation template to create: 
- Lambda function
- Lambda IAM role
- Secrets Manager Rotation Schedule
- Secrets Manager Secret 

## Requirements

1. An AWS account.

2. A Windows EC2 Instance with the SSM agent installed.  

3. An S3 bucket to store Lambda source code

## Deployment Steps 

1. Upload Lambda source code to an S3 bucket 
> aws s3 cp password-rotation/ s3://<s3-bucket-name>/Lambdas/ --recursive

2. Deploy the CloudFormation stack using the CLI command below: 
> $ aws cloudformation deploy \
--template <path_to_repo>/password-rotation.yaml \
--stack-name password-rotation \
--parameter-overrides EC2Id=<EC2Id> LambdaS3Bucket=<LambdaS3Bucket> LambdaS3BucketKey=<LambdaS3BucketKey> 