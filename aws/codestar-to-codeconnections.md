# 💭 AWS CodeStar Connections to AWS CodeConnections

Resources that have been created with `codestar-connections` in the ARN will not automatically be renamed to the new service prefix in the resource ARN.
Creating a new resource will create a resource that has the connections (`codeconnections`) service prefix.

**Problem**

```
Unable to use Connection: arn:aws:codestar-connections:<REGION>:<ACCOUNT-ID>:connection/<CONNECTION-ID>. The provided role does not have sufficient permissions.
```

**Possible Fix**

* Ensure the CodeStar Connection ARN is correct.
    * Make sure that the CodeStar Connection ARN is properly formatted using AWS CDK ARN Formatter.

        ```go
        awscdk.Arn_Format(
        &awscdk.ArnComponents{
            Resource:     jsii.String("connection"),
            Service:      jsii.String("codeconnections"),
            Account:      awscdk.Aws_ACCOUNT_ID(),
            ArnFormat:    awscdk.ArnFormat_SLASH_RESOURCE_NAME,
            Partition:    awscdk.Aws_PARTITION(),
            Region:       jsii.String(codestarRegion), // Region where CodeStar connection is located
            ResourceName: jsii.String(codestarID),     // The specific CodeStar connection ID
        }, stack)
        ```

* Update IAM policies for the new service prefix.
    1. Go to the **CodePipeline** in the AWS Management Console and select your pipeline.
    2. Go to the **Pipeline Settings** tab and look for a section called **Service role ARN**.
    3. Attach a policy like this to the IAM role:

        ```json
        {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Effect": "Allow",
                    "Action": [
                        "codestar-connections:UseConnection"
                    ],
                    "Resource": "arn:aws:codeconnections:<REGION>:<ACCOUNT-ID>:connection/<CONNECTION-ID>"
                }
            ]
        }
        ```

## 📋 Related Articles
* [Connections rename - Summary of changes](https://docs.aws.amazon.com/dtconsole/latest/userguide/rename.html)
* [Solving permissions error with AWS CodePipeline](https://haydnjmorris.medium.com/solving-permissions-error-with-aws-codepipeline-c93cfc000285)
* [Introducing AWS CodeConnections, formerly known as AWS CodeStar Connections](https://aws.amazon.com/about-aws/whats-new/2024/03/aws-codeconnections-formerly-codestar-connections/)