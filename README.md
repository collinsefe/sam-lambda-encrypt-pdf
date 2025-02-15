# CloudSkills PDF Encryption Lambda Stack

This repository contains a CloudFormation template to deploy a serverless architecture for encrypting PDFs uploaded to an S3 bucket. The stack uses AWS Lambda to automatically encrypt PDFs when they are uploaded to an S3 bucket.

## Architecture Overview

This template deploys the following resources:

- **AWS Lambda Function (CloudSkillsEncryptPDFFunction)**: A serverless Lambda function triggered by S3 events to encrypt PDFs.
- **S3 Buckets**:
  - **CloudSkillsPDFSourceBucket**: The source S3 bucket where unencrypted PDFs are uploaded.
  - **EncryptedPDFBucket**: The destination S3 bucket where the encrypted PDFs are stored.
  - **CollinsDemoBucket**: A demo S3 bucket for Collins' use.
  - **BimboDemoBucket**: A demo S3 bucket for Bimbo's use.

## Resources

### CloudSkillsEncryptPDFFunction

- **Type**: `AWS::Serverless::Function`
- **Properties**:
  - `FunctionName`: `CloudskillsEncryptedPDF-Function`
  - `Architectures`: `x86_64`
  - `CodeUri`: `./`
  - `Handler`: `lambda_function.lambda_handler`
  - `Runtime`: `python3.12`
  - `Timeout`: `15` seconds
  - `MemorySize`: `256` MB
  - `LoggingConfig`: Log format set to JSON.
  - `Policies`: Includes full access to S3 resources (`AmazonS3FullAccess`).
  - **Events**:
    - S3 event trigger: The Lambda function is triggered when a new PDF is uploaded to `CloudSkillsPDFSourceBucket` using the `s3:ObjectCreated:*` event.

### CloudSkillsPDFSourceBucket

- **Type**: `AWS::S3::Bucket`
- **Properties**:
  - `BucketName`: `cloudskillspdfsource-15022025`
  
  This is the source bucket where unencrypted PDF files are uploaded.

### EncryptedPDFBucket

- **Type**: `AWS::S3::Bucket`
- **Properties**:
  - `BucketName`: `cloudskillspdfsource-15022025-encrypted`
  
  This is the destination bucket for storing encrypted PDFs.

### CollinsDemoBucket

- **Type**: `AWS::S3::Bucket`
- **Properties**:
  - `BucketName`: `collinscloudskillsbucket-15022025`
  
  A demo bucket for Collins.

### BimboDemoBucket

- **Type**: `AWS::S3::Bucket`
- **Properties**:
  - `BucketName`: `bimbocloudskillsbucket-15022025`
  
  A demo bucket for Bimbo.

## Prerequisites

- AWS Account with the necessary permissions to create Lambda functions and S3 buckets.
- AWS CLI installed and configured.
- Serverless framework (optional but recommended for deployment).

## How to Deploy

1. **Clone the repository**:
   ```bash
   git clone https://github.com/collinsefe/sam-lambda-encrypt-pdf
   cd sam-lambda-encrypt-pdf
