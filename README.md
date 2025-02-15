# CloudSkills PDF Encryption Lambda Stack

One of the most common use cases for AWS Lambda is to perform file processing tasks. For example, you might use a Lambda function to automatically create PDF files from HTML files or images, or to create thumbnails when a user uploads an image.

In this example, we create an app which automatically encrypts PDF files when they are uploaded to an Amazon Simple Storage Service (Amazon S3) bucket. This stack includes the following resources:

- **An S3 bucket for users to upload PDF files**.
- **A Lambda function in Python** which reads the uploaded PDF file and creates an encrypted, password-protected version of it.
- **A second S3 bucket** where Lambda saves the encrypted PDF file.

## Architecture Overview

This template deploys the following resources:

- **AWS Lambda Function (CloudSkillsEncryptPDFFunction)**: A serverless Lambda function triggered by S3 events to encrypt PDF files.
- **S3 Buckets**:
  - **CloudSkillsPDFSourceBucket**: The source S3 bucket where unencrypted PDFs are uploaded by users.
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
- AWS SAM CLI installed. [Install AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-install.html).

## How to Deploy Using AWS SAM

1. **Clone the repository**:
    ```bash
   git clone https://github.com/collinsefe/sam-lambda-encrypt-pdf
   cd sam-lambda-encrypt-pdf
   ```

2. **Build the application**:
   Use the AWS SAM CLI to build the Lambda function and any other dependencies:
   ```bash
   sam build
   ```

3. **Deploy the application**:
   After building, deploy the stack using SAM CLI:
   ```bash
   sam deploy --guided
   ```

   The `--guided` flag will prompt you to configure deployment parameters, such as stack name, AWS region, and capabilities. Once you've answered the prompts, SAM will create and deploy the resources defined in your `template.yaml` file.

4. **Confirm deployment**:
   After the deployment is complete, SAM will output information about your deployed stack, including the Lambda function ARN and S3 bucket names.

## Testing the Function

1. Upload a PDF file to the `CloudSkillsPDFSourceBucket` via the AWS S3 Console or CLI.
2. The Lambda function will automatically trigger upon file upload and start the encryption process.
3. The encrypted PDF will be stored in the `EncryptedPDFBucket`.

## Notes

- Ensure the `CloudSkillsPDFSourceBucket` contains only PDFs or other files that can be encrypted.
- The Lambda function has a timeout of 15 seconds and a memory size of 256 MB, which should be sufficient for encrypting PDF files.
- The stack deploys demo buckets for `Collins` and `Bimbo` but these are optional and may be used for testing purposes.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

