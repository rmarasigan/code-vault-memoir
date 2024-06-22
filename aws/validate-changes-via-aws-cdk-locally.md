# 💭 Validate Changes via AWS CDK Locally

1. List all the stacks in your CDK application.

      ```bash
      dev@dev:~/your-project$ cdk list
      YourProjectStack
      YourProjectStack/YourProjectAppProduction/ApplicationAPIStack
      YourProjectStack/YourProjectAppProduction/ComputeStack
      YourProjectStack/YourProjectAppTest/ApplicationAPIStack
      YourProjectStack/YourProjectAppTest/ComputeStack
      ```

      <br />

      > **ℹ️ NOTE**
      >
      > You could use the alias:
      >
      > ```bash
      > cdk ls
      > ```
      >
      > This command will display a list of all stacks that have been defined in your CDK application, including the stack name, the stack environment, and stack status.

2. Display the specific stack differences between your local AWS CDK application and the deployed AWS CloudFormation stack.

      ```bash
      dev@dev:~/your-project$ cdk diff YourProjectStack/YourProjectAppTest/ComputeStack
      Stack YourProjectStack/YourProjectAppTest/ComputeStack
      Resources
      ┌───┬───────────┬───────────────────────────┬────────┬───────────────────────────────────────────────────────────┐
      │   │ Resource  │ Type                      │ Action │ Detail                                                    │
      ├───┼───────────┼───────────────────────────┼────────┼───────────────────────────────────────────────────────────┤
      │ 1 │ MyBucket  │ AWS::S3::Bucket           │ Modify │ Tags                                                      │
      │   │           │                           │        │ ├─ Environment: "Development" → "Test"                    │
      │   │           │                           │        │ └─ Owner: "TeamA" → "TeamB"                               │
      ├───┼───────────┼───────────────────────────┼────────┼───────────────────────────────────────────────────────────┤
      │ 2 │ NewTable  │ AWS::DynamoDB::Table      │ Create │ TableName: "TasksTable"                                   │
      │   │           │                           │        │ AttributeDefinitions: [ { AttributeName: "ID",            │
      │   │           │                           │        │ AttributeType: "S" } ]                                    │
      │   │           │                           │        │ KeySchema: [ { AttributeName: "ID", KeyType: "HASH" } ]   │
      │   │           │                           │        │ ProvisionedThroughput: { ReadCapacityUnits: 5,            │
      │   │           │                           │        │ WriteCapacityUnits: 5 }                                   │
      ├───┼───────────┼───────────────────────────┼────────┼───────────────────────────────────────────────────────────┤
      │ 3 │ OldBucket │ AWS::S3::Bucket           │ Delete │ BucketName: "OldBucket"                                   │
      └───┴───────────┴───────────────────────────┴────────┴───────────────────────────────────────────────────────────┘
      ```

3. You can locally execute the `cdk synth` command for the specific stack and generate the CloudFormation. Usually, you can detect errors already here but not all errors are caught immediately.

      ```bash
      dev@dev:~/your-project$ cdk synth YourProjectStack/YourProjectAppTest/ComputeStack
      Resources:
        TasksFilesBucket:
          Type: AWS::S3::Bucket
          Properties:
            Tags:
              - Key: Environment
                Value: Test
              - Key: Owner
                Value: TeamB

        TasksTable:
          Type: AWS::DynamoDB::Table
          Properties:
            TableName: NewTable
            AttributeDefinitions:
              - AttributeName: ID
                AttributeType: S
            KeySchema:
              - AttributeName: ID
                KeyType: HASH
            ProvisionedThroughput:
              ReadCapacityUnits: 5
              WriteCapacityUnits: 5

        OldBucket:
          Type: AWS::S3::Bucket
          DeletionPolicy: Delete
          UpdateReplacePolicy: Delete

      Outputs:
        MyBucketArn:
          Description: The ARN of the S3 bucket
          Value: !GetAtt TasksFilesBucket.Arn

        NewTableArn:
          Description: The ARN of the DynamoDB table
          Value: !GetAtt TasksTable.Arn
      ```

      <br />

      > **ℹ️ NOTE**
      >
      > Some people use the `cdk synth` with `deploy` because it is a **good practice**. 
      >
      > It is optional (though a good practice) to synthesize before deploying. The AWS CDK synthesizes your stack before each deployment.

## 📋 Related Articles
- [AWS CDK Toolkit](https://github.com/aws/aws-cdk/blob/main/packages/aws-cdk/README.md)
- [How to List Stacks in AWS CDK](https://blog.mikaeels.com/how-to-list-stacks-in-aws-cdk)
- [What does the AWS CDK Diff command do](https://blog.mikaeels.com/what-does-the-aws-cdk-diff-command-do)
- [What is the purpose of app.synth() in AWS CDK?](https://stackoverflow.com/questions/68434528/what-is-the-purpose-of-app-synth-in-aws-cdk)