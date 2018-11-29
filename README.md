# Centralized CloudTrail Logging CloudFormation templates
CloudFormation templates to simplify the configuration of a centralized CloudTrail logging account and allowing other accounts access.

## Features
- Centralized S3 Bucket with appropriate bucket policy for cross-account access
- Centralized KMS key used for CloudTrail with policy that allows cross-Account access
- Separate Local* and Centralized Trails
- Ability to choose various CloudTrail logging options (e.g. Write-Only vs. All events, Global vs. Region, etc.)

_("Local" in this case means the same AWS account that the CF stack is created in.)_

## Deployment
Feel free to modify the templates to have your desired default parameters to reduce configuration overhead.

The steps below are for the console, but could easily be replicated via the AWS CLI.

#### Security/Logging Account
1. Login to your Security/Logging AWS account
2. Create a New Stack in the CloudFormation console.
3. Upload the `logging-account-cloudtrail.yaml` file as the template.
4. Customize the parameters for your use case.
5. Deploy the Stack
6. Note the outputs for the KMS key ARN and S3 bucket name

#### Resource Accounts
1. If you haven't done so yet, follow the instructions above to deploy the centralized logging stack in your Security/Logging AWS account.
2. Login to the AWS Account you want to have log to the centralized bucket.
3. Create a New Stack in the CloudFormation console.
4. Upload the `other-accounts-cloudtrail.yaml` file as the template.
5. Copy the KMS key ARN and S3 bucket name from the Logging Account Stack
6. Customize other parameters for your use case.
7. Deploy the Stack

## Caveats/Limitations
- There are parameters for 10 AWS accounts logging to the centralized bucket. If you need more, please modify the template accordingly by adding additional parameters and referencing them in the proper resource definitions.
- There is currently no support for data events through this CF template (e.g., Lambda/S3 events)
- If you originally deploy the stack with encryption enabled and then disable it, the CloudTrail console still shows the Trail as using the KMS key. CloudFormation will do the update, and mark the keys for deletion, but it won't actually change the logs to be unencrypted. This might be a bug in CloudFormation.

## Future Work
Want to help out? Feel free to fork and create a pull request with added features. Some things that would be nice to add in the future:
- Lambda/S3 Data Events
- Option to push to CloudWatch Logs
- Options to publish to an SNS Topic